-- Table Value

local module = {}

local Signal = require(script.Parent.Signal)
local MiscUtil = require(script.Parent.MiscUtil)

local TableValue = {}

local function new(Data)
	local NewTableValue = {}
	
	for i, v in Data do
		if type(v) == "table" then
			NewTableValue[i] = new(v)
		end
	end
	
	NewTableValue.Data = Data or {}
	NewTableValue.Changed = Signal.NewSignal()
	
	return setmetatable(NewTableValue, TableValue)
end

local __newindex = function(self, Index, Value)
		if self[Index] ~= Value then
			self.Changed:Fire(Index, Value, self.Data[Index], self.Data)
			self.Data[Index] = if type(Value) == "table" then new(Value) else Value
		end
end

local __index = function(self, Index)
		return self.Data[Index]
end

local __len = function(self)
		return #self.Data	
end
	
local __iter = function(self)
		return next, self
end


TableValue.__newindex = __newindex
TableValue.__index = __index
TableValue.__len = __len
TableValue.__iter = __iter
TableValue.new = new


module.new = TableValue.new

function module.SetUniversalChangedEvent(Root, Callback: (Index: string, New: any, Old: any, Table: {}) -> nil)
    if not Root.Changed then return end
    
    Root.Changed:Connect(Callback)
    

    for _, v in Root do
        if type(v) == "table" then
			module.SetUniversalChangedEvent(v, Callback)
		end
	end
end


return module
