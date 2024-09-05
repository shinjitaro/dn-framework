-- Misc Util

local module = {}

local function GetTablePath(Root, Target: {Table: {}, Key: any}, CurrentPath)
	local CurrentPath = CurrentPath or {}
	
	for Key, Value in Root do
		table.insert(CurrentPath, Key)
        
        if Root == Target.Table and Root[Target.Key] then
            return CurrentPath
		elseif type(Value) == "table" then
			local path = GetTablePath(Value, Target, CurrentPath)
			
			if path then return path end
			
		end
	end
	
	table.remove(CurrentPath)
	
	return nil
end

local function UpdateNestedTable(Root, Path, Value)
	local Current = Root
	    
	for i = 1, #Path - 1 do
		Current = Current[Path[i]]
	end
	
	Current[Path[#Path]] = Value
end

local ValueInstanceMap = {
    string = Instance.new("StringValue"),
    number = Instance.new("NumberValue"),
    boolean = Instance.new("BoolValue"),
}

local function TableToFolder(Table: {}) : Folder
    local Folder = Instance.new("Folder")
    
    for i, v in Table do
        local ValueType = type(v)
        
        if ValueType == "table" then
            local SubFolder = TableToFolder(v)
            SubFolder.Name = i
            SubFolder.Parent = Folder
        else
            local Value = ValueInstanceMap[ValueType]:Clone()
            Value.Name = i
            Value.Value = v
            Value.Parent = Folder
        end
    end
    
    return Folder
end

local function FolderToTable(Folder: Folder) : {}
    local Table = {}
    
    for i, v in Folder:GetChildren() do
        if v:IsA("Folder") then
            Table[v.Name] = FolderToTable(v)
        else
            Table[v.Name] = v.Value
        end
    end
    
    return Table
end

module.GetTablePath = GetTablePath
module.UpdateNestedTable = UpdateNestedTable
module.TableToFolder = TableToFolder
module.FolderToTable = FolderToTable

return module
