-- Remotes Handler

local RunService = game:GetService("RunService")
local IsClient = RunService:IsClient()

local module = {}

local Remote = script:WaitForChild("RemoteEvent")

module.Events = {}

function module.HookEvent(Name, Callback)
	module.Events[Name] = Callback
end

function module.FireServer(Name, ...)
	Remote:FireServer(Name, ...)
end

function module.FireClient(Player, Name, ...)
	Remote:FireClient(Player, Name, ...)
end


if IsClient then
	Remote.OnClientEvent:Connect(function(Event, ...)
		if module.Events[Event] then
			module.Events[Event](...)
		end
	end)
	
else
	Remote.OnServerEvent:Connect(function(Player, Event, ...)
		if module.Events[Event] then
			module.Events[Event](Player, ...)
		end
	end)
end



return module
