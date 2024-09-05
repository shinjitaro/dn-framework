-- basic client side setup. This is a server script with the run context set to client.

 local DNFramework = require(script.Parent)

local UserInputService = game:GetService("UserInputService")
local Players = game:GetService("Players")

local Connections = {}

local function HumanoidDied(Humanoid: Humanoid)

end

local function CharacterRemoving(Character: Model)
	for _, Connection in Connections[Character] do
		Connection:Disconnect()
	end

	Connections[Character] = nil
end

local function CharacterAdded(Character: Model)
	Connections[Character] = {}
	
	local Humanoid = Character:WaitForChild("Humanoid") :: Humanoid
	
	Humanoid.Died:Once(function()
		print(Character, "Died")
		HumanoidDied(Humanoid)
	end)
end

local function PlayerRemoving(Player: Player)
	print(Player, "has left the game")
end

local function PlayerAdded(Player: Player)
	print(Player, "has joined the game")
	
	local Character = Player.Character
	
	if Character then
		task.defer(CharacterAdded, Character)
	end
	
	Player.CharacterAdded:Connect(CharacterAdded)
	Player.CharacterRemoving:Connect(CharacterRemoving)
end


local LocalPlayer = Players.LocalPlayer

Players.PlayerAdded:Connect(PlayerAdded)
Players.PlayerRemoving:Connect(PlayerRemoving)

for _, Player in Players:GetPlayers() do
	task.defer(PlayerAdded, Player)
end

