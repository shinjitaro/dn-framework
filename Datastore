local RunService = game:GetService("RunService")
local TableValue = require(script.Parent.TableValue)
local Remotes = require(script.Parent.Remotes)
local MiscUtil = require(script.Parent.MiscUtil)

local module = {}
module.DataFiles = {}
module.AutoSaveDelay = 60

local Players = game:GetService("Players")
local HttpService = game:GetService("HttpService")

local TableIds = {}


if RunService:IsClient() then
	local LocalPlayer = Players.LocalPlayer
	
	local ClientEvents = {
		InitialData = function(Data)
			module.DataFiles[LocalPlayer] = {
				Data = Data,
				Edit = TableValue.new(Data)
            }
            
            TableValue.SetUniversalChangedEvent(module.DataFiles[LocalPlayer].Edit, function(...)
                print(LocalPlayer.Name, "Client:", ...)
            end)

			print("DN Framework | Loaded Data for " .. LocalPlayer.Name, module.DataFiles[LocalPlayer])
		end,
		
		SendRequest = function(Data)
			print("DN Framework | " .. LocalPlayer.Name ..  " has sent initial data request")

			Remotes.FireServer("DataReplication", "InitialRequest")
		end,
		
	}
		
	Remotes.HookEvent("DataReplication", function(Path, Data, Value)
		if ClientEvents[Path] then
			ClientEvents[Path](Data)
        else
            MiscUtil.UpdateNestedTable(module.DataFiles[LocalPlayer].Edit, Path, Value)    
		end

	end)
end

if RunService:IsServer() then
	local DatastoreService = game:GetService("DataStoreService")
	local DataSave = "Game1"
	local MainDatastore = DatastoreService:GetDataStore(DataSave)
	module.DefaultData = require(script:WaitForChild("DefaultData"))


	module.LastAutosave = 0

	module.LoadData = function(Player: Player)
		local Data, Success, Error

		while not Success do
			Success, Error = pcall(function()
                Data = MainDatastore:GetAsync(Player.UserId)
                
			end)

			if not Success then
				task.wait(6) 
			end

			if not Players:FindFirstChild(Player.Name) then
				print("DN Framework | " .. Player.Name .. " left while loading data. Aborting load." )
				return
			end
        end
        

		if Success then
			if not Data then
				Data = module.DefaultData.GetDefaultData()
			end

			Data = module.DefaultData.ConsolidateData(Data)

            local Edit = TableValue.new(Data)
            
			module.DataFiles[Player] = {
				Data = Data,
				Edit = Edit
			}
			
            TableValue.SetUniversalChangedEvent(Edit, function(Index, New, Old, ValueTable)                
                local Path = MiscUtil.GetTablePath(
                    Data,
                    
                    {
                        Key = Index,
                        Table = ValueTable
                    }
                    
                )
                                
                Remotes.FireClient(Player, "DataReplication", Path, nil, New)
			end)

		else
			warn("DN Framework | Could not load data for "..Player.Name.." | "..Error)
		end

		print("DN Framework | Loaded data for "..Player.Name)
        Remotes.FireClient(Player, "DataReplication", "SendRequest")
        
		return true
	end

	module.SaveData = function(Player: Player, LastSave)	
		local DataFile = module.DataFiles[Player]

		if not DataFile then return end

		local Data = DataFile.Data
		
		if LastSave then module.DataFiles[Player] = nil end

		local Success, Error = pcall(function()
			MainDatastore:SetAsync(Player.UserId, Data)
		end)

		if not Success then
			warn("DN Framework | Could not save data for "..Player.Name.." | "..Error)
		end

		print("DN Framework | Saved data for "..Player.Name)

		return Success
	end

	module.AutoSave = function()
		if os.clock() - module.LastAutosave < module.AutoSaveDelay then return end

		module.LastAutosave = os.clock()

		for Player, Data in module.DataFiles do
			module.SaveData(Player)
		end

	end

	game:BindToClose(function()		
		for Player, Data in module.DataFiles do
			module.SaveData(Player, true)
		end

		print("DN Framework | Datastore | All data saved on server shutdown")
	end)

	RunService.Heartbeat:Connect(module.AutoSave)
	
	local FirstBatchSent = {}
	local function InitialDataRequest(Player, Event)
		if Event == "InitialRequest" and FirstBatchSent[Player] then return end
		FirstBatchSent[Player] = true
		
        local Data = module.DataFiles[Player].Data

		print("DN Framework | Replication | Sending initial data to ".. Player.Name)
		Remotes.FireClient(Player, "DataReplication", "InitialData", Data)
	end
	
	Remotes.HookEvent("DataReplication", function(Player, Event)
		InitialDataRequest(Player, Event)
	end)
	
	Players.PlayerRemoving:Connect(function(Player)
		if FirstBatchSent[Player] then FirstBatchSent[Player] = nil end
	end)
end



return module
