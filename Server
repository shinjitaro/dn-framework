-- This is the main server script. This is a server script with the RunContext set to Server

local DNFramework = require(script.Parent)

local Players = game:GetService("Players")


Players.PlayerAdded:Connect(function(Player: Player)
    DNFramework.Datastore.LoadData(Player)
    
    local leaderstats = DNFramework.MiscUtil.TableToFolder(DNFramework.Datastore.DataFiles[Player].Data.leaderstats)
    leaderstats.Name = "leaderstats"
    leaderstats.Parent = Player
    
    DNFramework.Datastore.DataFiles[Player].Edit.leaderstats.Changed:Connect(function(Index, New)
        leaderstats:FindFirstChild(Index).Value = New
    end)
end)

Players.PlayerRemoving:Connect(function(Player: Player)
    print(DNFramework.Datastore.DataFiles[Player].Data)
	DNFramework.Datastore.SaveData(Player)
end)

