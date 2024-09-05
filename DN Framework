-- the module

local RunService = game:GetService("RunService")

local module = {}

module.Remotes = require(script.Remotes)
module.Signal = require(script.Signal)
module.TableValue = require(script.TableValue)
module.Datastore = require(script.Datastore)
module.MiscUtil = require(script.MiscUtil)

for i, v in script:WaitForChild("Features"):GetChildren() do
	require(v)
end


return module
