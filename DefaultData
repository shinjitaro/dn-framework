-- Default Data, to be used with Datastore

local function GetDefaultData () : {PlayerData: {}, leaderstats: {}}
	return {
        leaderstats = {
            Wins = 0,
        },
	}	
end

local function ConsolidateData (Data, DefaultData)
	local DefaultData = DefaultData or GetDefaultData()
	
    if type(Data) ~= "table" then
		Data = DefaultData
	end
	
	for Index, Value in DefaultData do
		if Data[Index] == nil then
			Data[Index] = Value
		elseif type(Value) == "table" then
			ConsolidateData(Data[Index], Value)
		end
	end
	
	return Data
end



return {
	GetDefaultData = GetDefaultData,
	ConsolidateData = ConsolidateData
}
