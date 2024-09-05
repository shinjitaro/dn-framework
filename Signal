-- signal

local module = {}

local function Connect(self, Callback: (any?) -> any?)
	local Connection = {}
	
	Connection.Disconnect = function()
		self.Connections[Connection] = nil
	end
	
	self.Connections[Connection] = Callback
	
	return Connection
end

local function Once(self, Callback: (any?) -> any?)
	local OnceConnection = {}
	
	OnceConnection.Disconnect = function()
		self.OnceConnections[OnceConnection] = nil
	end
	
	self.OnceConnections[OnceConnection] = Callback
	
	return OnceConnection	
end

local function Fire(self, ...)
	for OnceConnection, Callback in self.OnceConnections do
		task.defer(Callback, ...)
		self[OnceConnection] = nil
	end
	
	for _, Callback in self.Connections do
		task.defer(Callback, ...)
	end
end

module.NewSignal = function()
	return {
		Connections = {},
		OnceConnections = {},
		Connect = Connect,
		Fire = Fire,
		Once = Once
	}
end

return module
