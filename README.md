<div align="center">
<h1>Dice Manager</h1>

By [Mullet Mafia Dev](https://www.roblox.com/groups/5018486/Mullet-Mafia-Dev#!/about) | [Download](https://www.roblox.com/library/5653863543/Dice-Manager) | [Source](https://github.com/Mullets-Gavin/DiceManager)
</div>

Dice Manager is an elegant solution to solving the ever-challenging memory-posed problem of connections. Utilizing Dice Manager, you can manage your connections, custom wrap calls, and create a queue task scheduler. Dice Manager provides all the barebone tools you can use to hook events & custom connections to, allowing you to avoid the hassle of `table.insert` for all of your event loggings by using the `:ConnectKey()` method.

## Documentation

### DiceManager.set
```lua
Manager.set(properties)
```
Set the properties of the manager:

```
properties = dictionary
properties['Debug'] = bool -- true (on) or false (off)
properties['RunService'] = 'Stepped' or 'Heartbeat'
```

### DiceManager.wait
```lua
.wait([time])
```
An accurate wait function that takes a given time after the time is met more accurately than `wait()`

*Example:*
```lua
local function run()
	print('running!')
end

Manager.wait(50) -- wait 50 seconds to run
run()
```

**Returns**
```lua
-- Stepped:
time -- The duration (in seconds) that RunService has been running for
step -- The time (in seconds) that has elapsed since the previous frame

-- Heartbeat:
step -- The time (in seconds) that has elapsed since the previous frame
```
The parameters of RunService.Stepped or RunService.Heartbeat are returned.

### DiceManager.wrap
```lua
.wrap(function)
```
Supply a function parameter to create a custom coroutine.wrap() function that acts the same except gives you a reliable stack trace when the function errors.

*Example:*
```lua
local function run()
	print('running!')
	print('break' + 100)
end

Manager.wrap(run)
```

### DiceManager.spawn
```lua
.spawn(function)
```
Supply a function parameter to create a custom coroutine.wrap() function that acts the same except you won't receive the errors caused, using pcall.

*Example:*
```lua
local function run()
	print('running!')
	print('the next line breaks, but the function wont print the error')
	print('break' + 100)
end

Manager.spawn(run)
```

### DiceManager.delay
```lua
.delay(time,function)
```
An accurate delay function that takes a given time & a function, and runs after the time is met more accurately than `wait()` & built-in `delay()`

*Example:*
```lua
local function run()
	print('running after 5 seconds passed')
end

Manager.delay(5,run)
```

### DiceManager.garbage
```lua
.garbage(time,instance)
```
An accurate delay function that takes a given time & an instance, and runs after the time is met more accurately than `wait()` & built-in `Debris:AddItem()`

*Example:*
```lua
local obj = instance.new('Part')
obj.Parent = workspace
obj.Anchored = true

Manager.garbage(5,obj)
```

### DiceManager:Connect
```lua
:Connect(function)
:Connect(RBXScriptConnection)
```
Supply a function or RBXScriptConnection type. Returns a dictionary with methods.

**Returns:**
```lua
control = dictionary
control:Fire(...)
control:Disconnect()
```
Calling `:Fire()` only works if the supplied parameter to `:Connect()` was a function. `:Disconnect()` disconnects all RBXScriptConnection or functions supplied in the `:Connect()` method. Optional parameters to `:Fire(...)` is allowed.

*Example:*
```lua
local function run(text)
	print(text)
end

local event = Manager:Connect(run)
event:Fire('fired')
```

### DiceManager:ConnectKey
```lua
:ConnectKey(key,function)
:ConnectKey(key,RBXScriptConnection)
```
Supply a key along with a function or RBXScriptConnection type. Returns a dictionary with methods. Locks the provided functions/signals to a key.

**Returns:**
```lua
control = dictionary
control:Fire(...)
control:Disconnect()
```
Calling `:Fire()` only works if the supplied parameter to `:ConnectKey()` was a function. `:Disconnect()` disconnects all RBXScriptConnection or functions supplied in the `:ConnectKey()` method. Optional parameters to `:Fire(...)` is allowed.

*Example:*
```lua
local function run(text)
	print(text)
end

local event = Manager:ConnectKey('Keepsake',run)
event:Fire('connection linked to a key')
```

### DiceManager:FireKey
```lua
:FireKey(key[,paramaters])
```
Fire all of the connections on a key with the given parameters.

*Example:*
```lua
Manager:ConnectKey('Keepsake',function(param)
	print('1:',param)
end)
Manager:ConnectKey('Keepsake',function(param)
	print('2:',param)
end)

Manager:FireKey('Keepsake','Running!')
```

### DiceManager:DisconnectKey
```lua
:DisconnectKey(key)
```
With the supplied key, disconnect all connections linked to the key.

*Example:*
```lua
Manager:ConnectKey('Keepsake',game:GetService('RunService').Heartbeat:Connect(function()
	print('running on key "Keepsake"!')
end))
Manager:ConnectKey('Keepsake',game:GetService('RunService').Heartbeat:Connect(function()
	print('running on key "Keepsake" as well omg!')
end))
game:GetService('RunService').Heartbeat:Wait()
Manager:DisconnectKey('Keepsake') -- disconnects all functions on the key
```

## DiceManager:Task
```lua
:Task([targetFPS])
```
Supply a target FPS to run on an independent channel or leave empty to run on the 60hz default (60 FPS). Create a task scheduler.

**Returns:**
```lua
control = dictionary
control:Queue(function)
control:Pause()
control:Resume()
control:Wait()
control:Enabled()
control:Disconnect()
```
Supply a function to `Queue()` to run a function in the order the function was passed in.

*Example:*
```lua
local scheduler = Manager:Task(30)
for index = 1,100 do
	scheduler:Queue(function()
		print('number:',index)
	end)
	if index == 50 then
		scheduler:Pause()
	end
end
print('Stopped at 50, starting again')
wait(2)
scheduler:Resume()
print('Finished')
scheduler:Disconnect()
```
