local TeleportService = game:GetService("TeleportService")
local customScripts = {
    ["Vapev4"] = "https://raw.githubusercontent.com/7GrandDadPGN/VapeV4ForRoblox/main/NewMainScript.lua",
    ["skyhub"] = "https://raw.githubusercontent.com/yofriendfromschool1/Sky-Hub/main/SkyHub.txt",
	["bypasser"] = "https://raw.githubusercontent.com/cheatplug/usercreated/refs/heads/main/main.lua",
	["infyield"] = "https://raw.githubusercontent.com/EdgeIY/infiniteyield/master/source",
    ["namelessadmin"] = "https://raw.githubusercontent.com/FilteringEnabled/NamelessAdmin/main/Source",
    ["dex"] = "https://cdn.wearedevs.net/scripts/Dex%20Explorer.txt",
	["rivials1"] = "https://get-8bit.freewebhostmost.com/scripts/?script=rivalsv3.lua",
	["rivials2"] = "https://api.luarmor.net/files/v3/loaders/31adf31e02665d1801c2e40d659fe00d.lua",
	["bladeb1"] = "https://api.luarmor.net/files/v3/loaders/79ab2d3174641622d317f9e234797acb.lua",
	["bladeb2"] = "https://rawscripts.net/raw/XMAS-Blade-Ball-OPEN-SOURCE-Simple-Proximity-Auto-Parry-25405",
	["fnfaplay"] = "https://pastebin.com/raw/dcyuEgyK",
	["graph"] = "https://pastebin.com/raw/MCJ57uC3",
}


local gameList = {
    ["mm2"] = 142823291,
    ["babft"] = 537413528,
    ["break in 2"] = 13864661000,
    ["break in 1"] = 3851622790,
	["ez-game-on-roblox"] = 16057679366,
	["blox fruits"] = 2753915549,
	["blox burg"] = 185655149,
	["jailbreak"] = 606849621,
	["fisch"] = 16732694052,
}

local links = {
    ["discord"] = "https://discord.gg/Tkkbt94nxH",
}

local Trails = {
    ["rainbow"] = Color3.new(1, 0, 0), -- Red start (will cycle through colors)
    ["red"] = Color3.fromRGB(255, 0, 0),
    ["blue"] = Color3.fromRGB(0, 0, 255),
    ["black"] = Color3.fromRGB(0, 0, 0),
    ["green"] = Color3.fromRGB(0, 255, 0),
    ["yellow"] = Color3.fromRGB(255, 255, 0),
    ["white"] = Color3.fromRGB(255, 255, 255),
    ["purple"] = Color3.fromRGB(128, 0, 128),
    ["pink"] = Color3.fromRGB(255, 105, 180),
    ["orange"] = Color3.fromRGB(255, 165, 0),
    ["cyan"] = Color3.fromRGB(0, 255, 255),
    ["magenta"] = Color3.fromRGB(255, 0, 255),
    ["lime"] = Color3.fromRGB(50, 205, 50),
    ["silver"] = Color3.fromRGB(192, 192, 192),
    ["gold"] = Color3.fromRGB(255, 215, 0)
}

-- Helper function to get table keys since table.keys isn't available in Roblox Lua
local function getKeys(tbl)
    local keys = {}
    for key, _ in pairs(tbl) do
        table.insert(keys, key)
    end
    return keys
end

local L_1_ = game:GetService("Players")
local L_2_ = game:GetService("ReplicatedStorage")
local L_3_ = game:GetService("UserInputService")
local L_4_ = L_1_.LocalPlayer
_G.currentPrefix = _G.currentPrefix or "!"
local L_5_ = _G.currentPrefix



-- Wait for player to be fully loaded
if not L_4_ then
	L_4_ = L_1_.LocalPlayer
	while not L_4_ do
		L_1_:GetPropertyChangedSignal("LocalPlayer"):Wait()
		L_4_ = L_1_.LocalPlayer
	end
end

-- Get current date/time and server info
local function L_6_func()
	local L_14_ = os.date("*t")
	return string.format("%02d/%02d/%d", L_14_.month, L_14_.day, L_14_.year), string.format("%02d:%02d:%02d", L_14_.hour, L_14_.min, L_14_.sec)
end

local L_7_, L_8_ = L_6_func()
local L_9_ = {
	["Username"] = L_4_.Name,
	["Display Name"] = L_4_.DisplayName,
	["Date"] = L_7_,
	["Time"] = L_8_,
	["Place Name"] = game:GetService("MarketplaceService"):GetProductInfo(game.PlaceId).Name,
	["Place ID"] = game.PlaceId,
	["Job ID"] = game.JobId
}

-- Print server information
print("\n=== Player & Server Information ===")
for L_15_forvar0, L_16_forvar1 in pairs(L_9_) do
	print(string.format("%s: %s", L_15_forvar0, tostring(L_16_forvar1)))
end
print("================================\n")

local L_10_ = {} -- Admins table
local L_11_ = "Commands: !speed, !jump, !fly, !reset, !respawn, !admin, !revoke, !bring, !tp, !crash, !noclip, !unnoclip, !lag, !unlag, !esp, !user, !stop, !unstop, !userintell, !list, !zoom, !wair, !unwair !airjump, !unairjump, !scripts, !relog, !game, !link, !cf, !graph, !ungraph, !graph2, !ungraph2, !trail, !untrail, !setprefix"  -- Changed + to ..

-- Commands table
local L_12_ = {
	["speed"] = function(L_17_arg0)
		local L_18_ = tonumber(L_17_arg0[1]) or 16
		if L_4_.Character and L_4_.Character:FindFirstChild("Humanoid") then
			L_4_.Character.Humanoid.WalkSpeed = L_18_
			print("Speed changed to: " .. L_18_)
		end
	end,
	["jump"] = function(L_19_arg0)
		local L_20_ = tonumber(L_19_arg0[1]) or 50
		if L_4_.Character and L_4_.Character:FindFirstChild("Humanoid") then
			L_4_.Character.Humanoid.JumpPower = L_20_
			print("Jump power changed to: " .. L_20_)
		end
	end,
	
	["fly"] = function(args)
		local Players = game:GetService("Players")
		local RunService = game:GetService("RunService")
	
		local player = Players.LocalPlayer
		local character = player.Character or player.CharacterAdded:Wait()
	
		local flySpeed = tonumber(args[1]) or 50 -- Default speed is 50 if not specified
		local riseSpeed = 10
		local initialRiseTime = 1
		local turningSpeed = 0.2
	
		-- Prevent duplicate activation
		if _G.flying then
			game:GetService("StarterGui"):SetCore("ChatMakeSystemMessage", {
				Text = "[FLY] You are already flying!",
				Color = Color3.fromRGB(255, 255, 0)
			})
			return
		end
	
		_G.flying = true
		character.Humanoid.PlatformStand = true
	
		local bodyVelocity = Instance.new("BodyVelocity")
		bodyVelocity.MaxForce = Vector3.new(100000, 100000, 100000)
		bodyVelocity.Velocity = Vector3.new(0, 0, 0)
		bodyVelocity.Parent = character.HumanoidRootPart
	
		local bodyGyro = Instance.new("BodyGyro")
		bodyGyro.MaxTorque = Vector3.new(100000, 100000, 100000)
		bodyGyro.CFrame = character.HumanoidRootPart.CFrame
		bodyGyro.Parent = character.HumanoidRootPart
	
		_G.bodyVelocity = bodyVelocity
		_G.bodyGyro = bodyGyro
	
		-- Initial rise effect
		local startTime = tick()
		while tick() - startTime < initialRiseTime do
			bodyVelocity.Velocity = Vector3.new(0, riseSpeed, 0)
			RunService.RenderStepped:Wait()
		end
	
		-- Flying logic
		_G.flyConnection = RunService.RenderStepped:Connect(function()
			if _G.flying then
				local moveDirection = character.Humanoid.MoveDirection * flySpeed
				local camLookVector = workspace.CurrentCamera.CFrame.LookVector
	
				if moveDirection.Magnitude > 0 then
					if camLookVector.Y > 0.2 then
						moveDirection = moveDirection + Vector3.new(0, camLookVector.Y * flySpeed, 0)
					elseif camLookVector.Y < -0.2 then
						moveDirection = moveDirection + Vector3.new(0, camLookVector.Y * flySpeed, 0)
					end
				else
					moveDirection = Vector3.new(0, 0, 0) -- Stay in place when not moving
				end
	
				bodyVelocity.Velocity = moveDirection
	
				local tiltAngle = 40
				local tiltFactor = moveDirection.Magnitude / flySpeed
				local tiltDirection = 1
	
				if workspace.CurrentCamera.CFrame:VectorToObjectSpace(moveDirection).Z < 0 then
					tiltDirection = -1
				end
	
				local tiltCFrame = CFrame.Angles(math.rad(tiltAngle) * tiltFactor * tiltDirection, 0, 0)
				local targetCFrame = CFrame.new(character.HumanoidRootPart.Position, character.HumanoidRootPart.Position + camLookVector) * tiltCFrame
				bodyGyro.CFrame = bodyGyro.CFrame:Lerp(targetCFrame, turningSpeed)
			end
		end)
	
		game:GetService("StarterGui"):SetCore("ChatMakeSystemMessage", {
			Text = "[FLY] You are now flying at speed " .. flySpeed,
			Color = Color3.fromRGB(0, 255, 255)
		})
	end,
	
	["unfly"] = function(args)
		if not _G.flying then
			game:GetService("StarterGui"):SetCore("ChatMakeSystemMessage", {
				Text = "[FLY] You are not flying!",
				Color = Color3.fromRGB(255, 255, 0)
			})
			return
		end
	
		_G.flying = false
		local player = game.Players.LocalPlayer
		local character = player.Character or player.CharacterAdded:Wait()
	
		character.Humanoid.PlatformStand = false
	
		if _G.bodyVelocity then _G.bodyVelocity:Destroy() end
		if _G.bodyGyro then _G.bodyGyro:Destroy() end
		if _G.flyConnection then _G.flyConnection:Disconnect() end
	
		game:GetService("StarterGui"):SetCore("ChatMakeSystemMessage", {
			Text = "[FLY] You are no longer flying.",
			Color = Color3.fromRGB(255, 100, 100)
		})
	end,
	
	["reset"] = function(L_26_arg0)
		if L_4_.Character and L_4_.Character:FindFirstChild("Humanoid") then
			L_4_.Character.Humanoid.WalkSpeed = 16
			L_4_.Character.Humanoid.JumpPower = 50
			L_4_.Character.Humanoid.PlatformStand = false
			print("Reset all settings")
		end
	end,
	["respawn"] = function(L_27_arg0)
		if L_4_.Character and L_4_.Character:FindFirstChild("Humanoid") then
			L_4_.Character.Humanoid.Health = 0
			task.wait(0.5)
			L_4_:LoadCharacter()
			print("Player respawned")
		end
	end,
	["admin"] = function(L_28_arg0)
		if L_28_arg0[1] then
			local L_29_ = L_1_:FindFirstChild(L_28_arg0[1])
			if L_29_ then
				L_10_[L_29_.Name] = true
				game:GetService("StarterGui"):SetCore("ChatMakeSystemMessage", {
					Text = "[ADMIN] You have been given admin. " .. L_11_,  -- Changed + to ..
					Color = Color3.fromRGB(255, 255, 0),
					To = L_29_
				})
				print(L_29_.Name .. " was given admin permissions")
			end
		end
	end,
	["revoke"] = function(L_30_arg0)
		if L_30_arg0[1] then
			local L_31_ = L_1_:FindFirstChild(L_30_arg0[1])
			if L_31_ and L_10_[L_31_.Name] then
				L_10_[L_31_.Name] = nil
				game:GetService("StarterGui"):SetCore("ChatMakeSystemMessage", {
					Text = "[ADMIN] Your admin permissions have been revoked",
					Color = Color3.fromRGB(255, 0, 0),
					To = L_31_
				})
				print(L_31_.Name .. " had admin permissions revoked")
			end
		end
	end,
	["bring"] = function(L_32_arg0)
		if L_32_arg0[1] then
			local L_33_ = L_1_:FindFirstChild(L_32_arg0[1])
			if L_33_ and L_10_[L_4_.Name] then
				if L_33_.Character and L_33_.Character:FindFirstChild("HumanoidRootPart") then
					local L_34_ = L_4_.Character.HumanoidRootPart.CFrame
					L_33_.Character:SetPrimaryPartCFrame(L_34_)
					print("Brought " .. L_33_.Name)
				end
			end
		end
	end,
	["tp"] = function(L_35_arg0)
		if L_35_arg0[1] then
			local L_36_ = L_1_:FindFirstChild(L_35_arg0[1])
			if L_36_ and L_36_.Character and L_36_.Character:FindFirstChild("HumanoidRootPart") then
				if L_4_.Character and L_4_.Character:FindFirstChild("HumanoidRootPart") then
					local L_37_ = L_36_.Character.HumanoidRootPart.CFrame
					L_4_.Character:SetPrimaryPartCFrame(L_37_)
					print("Teleported to " .. L_36_.Name)
				end
			else
				print("Target player not found or not loaded")
			end
		end
	end,
	["crash"] = function(L_38_arg0)
		print("Initiating crash sequence...")
        
        -- Create a large number of instances to force a crash
		local function L_39_func()
			local L_40_ = {}
			for L_41_forvar0 = 1, 999999 do
				local L_42_ = Instance.new("Part")
				L_42_.Position = Vector3.new(math.huge, math.huge, math.huge)
				L_42_.Parent = workspace
				table.insert(L_40_, L_42_)
			end
		end
		L_39_func()
	end,

    -- Add these inside your commands table
	["noclip"] = function(L_43_arg0)
		local L_44_ = L_4_.Character
		if L_44_ then
			local L_45_ = L_44_:GetDescendants()
			for L_46_forvar0, L_47_forvar1 in ipairs(L_45_) do
				if L_47_forvar1:IsA("BasePart") then
					L_47_forvar1.CanCollide = false
				end
			end
			print("Noclip enabled")
        
        -- Set up connection to keep noclip active
			if not _G.noclipConnection then
				_G.noclipConnection = game:GetService("RunService").Stepped:Connect(function()
					for L_48_forvar0, L_49_forvar1 in ipairs(L_44_:GetDescendants()) do
						if L_49_forvar1:IsA("BasePart") then
							L_49_forvar1.CanCollide = false
						end
					end
				end)
			end
		end
	end,
	["unnoclip"] = function(L_50_arg0)
		local L_51_ = L_4_.Character
		if L_51_ then
			local L_52_ = L_51_:GetDescendants()
			for L_53_forvar0, L_54_forvar1 in ipairs(L_52_) do
				if L_54_forvar1:IsA("BasePart") then
					L_54_forvar1.CanCollide = true
				end
			end
        
        -- Disconnect noclip connection
			if _G.noclipConnection then
				_G.noclipConnection:Disconnect()
				_G.noclipConnection = nil
			end
			print("Noclip disabled")
		end
	end,

	["lag"] = function(L_arg0)
		local L_char = L_4_.Character
		if L_char and L_char:FindFirstChild("HumanoidRootPart") then
			local L_root = L_char.HumanoidRootPart
	
			if _G.lagConnection then
				print("Lag is already enabled!")
				return
			end
	
			print("Lag simulation enabled")
	
			-- Create lag effect
			_G.lagConnection = game:GetService("RunService").Heartbeat:Connect(function()
				if L_char and L_char:FindFirstChild("HumanoidRootPart") then
					local L_originalPos = L_root.Position
					local L_offset = Vector3.new(math.random(-2, 2), 0, math.random(-2, 2))
					
					-- Randomly teleport between original and offset position
					if math.random(1, 2) == 1 then
						L_root.CFrame = CFrame.new(L_originalPos + L_offset)
					else
						L_root.CFrame = CFrame.new(L_originalPos)
					end
				end
			end)
		end
	end,
	
	["unlag"] = function(L_arg0)
		if _G.lagConnection then
			_G.lagConnection:Disconnect()
			_G.lagConnection = nil
			print("Lag simulation disabled")
		else
			print("Lag is not active!")
		end
	end,
	
	["stop"] = function(L_61_arg0)
		local L_62_ = L_4_.Character
		if L_62_ then
        -- Get all parts of the character
			local L_63_ = L_62_:GetDescendants()
			for L_65_forvar0, L_66_forvar1 in ipairs(L_63_) do
				if L_66_forvar1:IsA("BasePart") then
					L_66_forvar1.Anchored = true
				end
			end
        
        -- Handle humanoid states
			local L_64_ = L_62_:FindFirstChild("Humanoid")
			if L_64_ then
				L_64_.PlatformStand = true
			end
        
        -- Set up connection to maintain anchor
			if not _G.anchorConnection then
				_G.anchorConnection = L_62_.DescendantAdded:Connect(function(L_67_arg0)
					if L_67_arg0:IsA("BasePart") then
						L_67_arg0.Anchored = true
					end
				end)
			end
			print("Character anchored - You are now immobile")
		end
	end,
	["unstop"] = function(L_68_arg0)
		local L_69_ = L_4_.Character
		if L_69_ then
        -- Get all parts of the character
			local L_70_ = L_69_:GetDescendants()
			for L_72_forvar0, L_73_forvar1 in ipairs(L_70_) do
				if L_73_forvar1:IsA("BasePart") then
					L_73_forvar1.Anchored = false
				end
			end
        
        -- Handle humanoid states
			local L_71_ = L_69_:FindFirstChild("Humanoid")
			if L_71_ then
				L_71_.PlatformStand = false
			end
        
        -- Clean up connection
			if _G.anchorConnection then
				_G.anchorConnection:Disconnect()
				_G.anchorConnection = nil
			end
			print("Character unanchored - You can move again")
		end
	end,

-- Add to your commands table
	["esp"] = function(L_74_arg0)
    -- Toggle ESP state
		_G.espEnabled = not _G.espEnabled
    
    -- Remove existing ESP
		for L_75_forvar0, L_76_forvar1 in pairs(workspace:GetChildren()) do
			if L_76_forvar1.Name == "ESP_Highlight" then
				L_76_forvar1:Destroy()
			end
		end
		if _G.espEnabled then
        -- Create ESP function
			_G.espConnection = game:GetService("RunService").RenderStepped:Connect(function()
				for L_77_forvar0, L_78_forvar1 in pairs(L_1_:GetPlayers()) do
					if L_78_forvar1 ~= L_4_ and L_78_forvar1.Character and L_78_forvar1.Character:FindFirstChild("HumanoidRootPart") then
                    
                    -- Check for existing ESP
						local L_79_ = L_78_forvar1.Character:FindFirstChild("ESP_Highlight")
						if not L_79_ then
                        -- Create ESP elements
							local L_80_ = Instance.new("Highlight")
							L_80_.Name = "ESP_Highlight"
							L_80_.FillColor = Color3.fromRGB(255, 0, 0)
							L_80_.OutlineColor = Color3.fromRGB(255, 255, 255)
							L_80_.FillTransparency = 0.5
							L_80_.OutlineTransparency = 0
							L_80_.Parent = L_78_forvar1.Character
                        
                        -- Add name ESP
							local L_81_ = Instance.new("BillboardGui")
							L_81_.Name = "ESP_Highlight"
							L_81_.AlwaysOnTop = true
							L_81_.Size = UDim2.new(0, 100, 0, 40)
							L_81_.StudsOffset = Vector3.new(0, 3, 0)
							L_81_.Parent = L_78_forvar1.Character.Head
							local L_82_ = Instance.new("TextLabel")
							L_82_.BackgroundTransparency = 1
							L_82_.Size = UDim2.new(1, 0, 1, 0)
							L_82_.Text = L_78_forvar1.Name
							L_82_.TextColor3 = Color3.fromRGB(255, 255, 255)
							L_82_.TextStrokeTransparency = 0
							L_82_.TextStrokeColor3 = Color3.fromRGB(0, 0, 0)
							L_82_.Parent = L_81_
						end
					end
				end
			end)
			print("ESP enabled")
		else
        -- Disable ESP
			if _G.espConnection then
				_G.espConnection:Disconnect()
				_G.espConnection = nil
			end
			print("ESP disabled")
		end
	end,

-- Add this to your commands table
	["user"] = function(L_83_arg0)
		if # L_83_arg0 < 1 then
			print("Usage: !user [display name]")
			return
		end
		local L_84_ = table.concat(L_83_arg0, " "):lower()
		local L_85_ = false
		for L_86_forvar0, L_87_forvar1 in pairs(L_1_:GetPlayers()) do
			if L_87_forvar1.DisplayName:lower() == L_84_ then
            -- Set clipboard using Synapse X or other executor's set clipboard function
				if setclipboard then
					setclipboard(L_87_forvar1.Name)
					print("Copied username: " .. L_87_forvar1.Name)
                
                -- Show notification
					game:GetService("StarterGui"):SetCore("ChatMakeSystemMessage", {
						Text = "[USER] Username copied: " .. L_87_forvar1.Name,
						Color = Color3.fromRGB(0, 255, 0)
					})
				else
					print("Username found but clipboard function not available: " .. L_87_forvar1.Name)
				end
				L_85_ = true
				break
			end
		end
		if not L_85_ then
			print("No player found with display name: " .. L_84_)
		end
	end,
	["userintell"] = function(L_88_arg0)
		if # L_88_arg0 < 1 then
			print("Usage: !userintell [username]")
			return
		end
		local L_89_ = L_88_arg0[1]
		local L_90_ = L_1_:FindFirstChild(L_89_)
		if not L_90_ then
			print("Player not found: " .. L_89_)
			return
		end
    
    -- Get friend count using GetFriendsAsync with better error handling
		local function L_91_func(L_97_arg0)
			local L_98_, L_99_ = 0, 0
			local L_100_, L_101_ = pcall(function()
				local L_102_ = L_1_:GetFriendsAsync(L_97_arg0)
				while true do
					local L_103_ = L_102_:GetCurrentPage()
					if not L_103_ then
						break
					end
					L_98_ = L_98_ + # L_103_
					for L_105_forvar0, L_106_forvar1 in ipairs(L_103_) do
						if L_106_forvar1.IsOnline then
							L_99_ = L_99_ + 1
						end
					end
					if L_102_.IsFinished then
						break
					end
					local L_104_ = pcall(function()
						L_102_:AdvanceToNextPageAsync()
					end)
					if not L_104_ then
						break
					end
				end
			end)
			return L_98_, L_99_
		end
    
    -- Safely get user info
		local L_92_ = "Unknown"
		pcall(function()
			local L_107_, L_108_ = pcall(function()
				return game:GetService("UserService"):GetUserInfosByUserIdsAsync({
					L_90_.UserId
				})[1]
			end)
			if L_107_ and L_108_ then
				L_92_ = tostring(L_108_.IsPremium)
			end
		end)
    
    -- Calculate join date
		local L_93_ = "Unknown"
		pcall(function()
			local L_109_ = os.time() - (L_90_.AccountAge * 86400)
			L_93_ = os.date("%Y-%m-%d", L_109_)
		end)
    
    -- Get friend counts safely
		local L_94_, L_95_ = L_91_func(L_90_.UserId)
    
    -- Print intel report
		local L_96_ = {
			"\n=== Player Intelligence Report ===",
			"Username: " .. L_90_.Name,
			"Display Name: " .. L_90_.DisplayName,
			"User ID: " .. L_90_.UserId,
			"Account Age: " .. L_90_.AccountAge .. " days",
			"Total Friends: " .. L_94_,
			"Online Friends: " .. L_95_,
			"Join Date: " .. L_93_,
			"Premium: " .. L_92_,
			"============================="
		}
		for L_110_forvar0, L_111_forvar1 in ipairs(L_96_) do
			print(L_111_forvar1)
		end
    
    -- Show chat message
		pcall(function()
			game:GetService("StarterGui"):SetCore("ChatMakeSystemMessage", {
				Text = string.format("[INTEL] %s (@%s) | Friends: %d (%d online) | Age: %d days", L_90_.DisplayName, L_90_.Name, L_94_, L_95_, L_90_.AccountAge),
				Color = Color3.fromRGB(255, 255, 0)
			})
		end)
	end,

	["list"] = function()
		-- Print to console
		print("\n=== Available Commands ===")
		print(L_11_)
		print("=======================\n")
		
		-- Split the command list into multiple chat messages to avoid length limits
		local commandList = L_11_:gsub("Commands: ", "")
		local commands = {}
		for cmd in commandList:gmatch("![%w]+") do
			table.insert(commands, cmd)
		end
		
		-- Send multiple chat messages with 5 commands each
		for i = 1, #commands, 5 do
			local slice = {}
			for j = i, math.min(i + 4, #commands) do
				table.insert(slice, commands[j])
			end
			game:GetService("StarterGui"):SetCore("ChatMakeSystemMessage", {
				Text = "[COMMANDS] " .. table.concat(slice, ", "),
				Color = Color3.fromRGB(0, 255, 255)
			})
		end
	end,

["zoom"] = function(L_arg0)
    local defaultZoom = 12.5  -- Roblox's default camera zoom distance
    local zoomDistance = defaultZoom
    
    if L_arg0[1] then
        if L_arg0[1]:lower() == "def" then
            zoomDistance = defaultZoom
        else
            zoomDistance = tonumber(L_arg0[1]) or defaultZoom
        end
    end
    
    -- Get player's camera
    local camera = workspace.CurrentCamera
    if camera then
        -- Adjust zoom distance
        L_4_.CameraMaxZoomDistance = zoomDistance
        L_4_.CameraMode = Enum.CameraMode.Classic
        
        print("Camera zoom set to: " .. zoomDistance)
        
        -- Show notification
        game:GetService("StarterGui"):SetCore("ChatMakeSystemMessage", {
            Text = "[ZOOM] Camera zoom distance set to " .. zoomDistance,
            Color = Color3.fromRGB(0, 255, 255)
        })
    end
end,

["wair"] = function(L_arg0)
    if not _G.airParts then _G.airParts = {} end
    
    if not _G.airConnection then
        _G.airConnection = game:GetService("RunService").RenderStepped:Connect(function()
            if L_4_.Character and L_4_.Character:FindFirstChild("HumanoidRootPart") then
                local pos = L_4_.Character.HumanoidRootPart.Position
                local part = Instance.new("Part")
                part.Size = Vector3.new(5, 0.1, 5)
                part.Anchored = true
                part.Transparency = 0.7
                part.BrickColor = BrickColor.new("Really light blue")
                part.Position = Vector3.new(pos.X, pos.Y - 3.5, pos.Z)
                part.Parent = workspace
                table.insert(_G.airParts, part)
                
                -- Remove old parts
                if #_G.airParts > 10 then
                    _G.airParts[1]:Destroy()
                    table.remove(_G.airParts, 1)
                end
            end
        end)
        print("Air walking enabled")
    end
end,

["unwair"] = function(L_arg0)
    if _G.airConnection then
        _G.airConnection:Disconnect()
        _G.airConnection = nil
        
        -- Clean up existing air parts
        if _G.airParts then
            for _, part in ipairs(_G.airParts) do
                part:Destroy()
            end
            _G.airParts = {}
        end
        print("Air walking disabled")
    end
end,

["airjump"] = function(L_arg0)
    if not _G.airJumpConnection then
        _G.airJumpConnection = game:GetService("UserInputService").JumpRequest:Connect(function()
            if L_4_.Character and L_4_.Character:FindFirstChild("Humanoid") then
                L_4_.Character.Humanoid:ChangeState(Enum.HumanoidStateType.Jumping)
            end
        end)
        print("Air jumping enabled")
    end
end,

["unairjump"] = function(L_arg0)
    if _G.airJumpConnection then
        _G.airJumpConnection:Disconnect()
        _G.airJumpConnection = nil
        print("Air jumping disabled")
    end
end,

["scripts"] = function(L_arg0)
    if #L_arg0 < 1 then
        print("Usage: !scripts [list/run] [script_name]")
        return
    end

    local action = L_arg0[1]:lower()

    if action == "list" then
        print("\n=== Available Scripts ===")
        for scriptName, _ in pairs(customScripts) do
            print(scriptName)
        end
        print("========================\n")

        -- Send chat messages with script names
        local scriptNames = {}
        for scriptName, _ in pairs(customScripts) do
            table.insert(scriptNames, scriptName)
        end
        game:GetService("StarterGui"):SetCore("ChatMakeSystemMessage", {
            Text = "[SCRIPTS] Available: " .. table.concat(scriptNames, ", "),
            Color = Color3.fromRGB(0, 255, 255)
        })
    
    elseif action == "run" and L_arg0[2] then
        local scriptName = L_arg0[2]:lower()
        if customScripts[scriptName] then
            local scriptURL = customScripts[scriptName]

            -- Load and execute the script
            local success, scriptCode = pcall(function()
                return game:HttpGet(scriptURL)
            end)

            if success then
                local runSuccess, err = pcall(function()
                    loadstring(scriptCode)()
                end)

                if runSuccess then
                    print("Successfully executed script: " .. scriptName)
                    game:GetService("StarterGui"):SetCore("ChatMakeSystemMessage", {
                        Text = "[SCRIPTS] Successfully executed: " .. scriptName,
                        Color = Color3.fromRGB(0, 255, 0)
                    })
                else
                    print("Error executing script: " .. err)
                end
            else
                print("Failed to load script: " .. scriptName)
            end
        else
            print("Script not found! Use !scripts list to see available scripts.")
        end
    else
        print("Invalid usage. Try !scripts list or !scripts run [script_name]")
    end
end,

["relog"] = function()
    local placeId = game.PlaceId -- Get current game's place ID
    local jobId = game.JobId -- Get current server's Job ID

    if placeId and jobId then
        print("Rejoining the same server...")
        
        -- Send system message
        game:GetService("StarterGui"):SetCore("ChatMakeSystemMessage", {
            Text = "[RELOG] Rejoining the same server...",
            Color = Color3.fromRGB(0, 255, 255)
        })
        
        -- Rejoin using TeleportService
        local success, err = pcall(function()
            TeleportService:TeleportToPlaceInstance(placeId, jobId, game.Players.LocalPlayer)
        end)

        if not success then
            print("Failed to rejoin: " .. err)
            game:GetService("StarterGui"):SetCore("ChatMakeSystemMessage", {
                Text = "[ERROR] Failed to rejoin: " .. err,
                Color = Color3.fromRGB(255, 0, 0)
            })
        end
    else
        print("Error: Could not retrieve server info.")
        game:GetService("StarterGui"):SetCore("ChatMakeSystemMessage", {
            Text = "[ERROR] Could not retrieve server info.",
            Color = Color3.fromRGB(255, 0, 0)
        })
    end
end,

["game"] = function(L_arg0)
    if #L_arg0 < 1 then
        print("Usage: !game [list/copy/go] [game_name/game_id]")
        return
    end

    local action = L_arg0[1]:lower()

    if action == "list" then
        print("\n=== Available Games ===")
        for gameName, gameId in pairs(gameList) do
            print(gameName .. " - ID: " .. gameId)
        end
        print("========================\n")

        -- Send chat messages with game names
        local gameNames = {}
        for gameName, _ in pairs(gameList) do
            table.insert(gameNames, gameName)
        end
        game:GetService("StarterGui"):SetCore("ChatMakeSystemMessage", {
            Text = "[GAMES] Available: " .. table.concat(gameNames, ", "),
            Color = Color3.fromRGB(0, 255, 255)
        })

    elseif action == "copy" and L_arg0[2] then
        -- Recombine arguments to allow spaces in game names
        table.remove(L_arg0, 1) -- Remove "copy"
        local gameName = table.concat(L_arg0, " "):lower()

        if gameList[gameName] then
            local gameId = tostring(gameList[gameName])

            if setclipboard then
                setclipboard(gameId)
                print("Copied Game ID: " .. gameId)

                game:GetService("StarterGui"):SetCore("ChatMakeSystemMessage", {
                    Text = "[GAMES] Game ID copied: " .. gameId,
                    Color = Color3.fromRGB(0, 255, 0)
                })
            else
                print("Clipboard function not available! Game ID: " .. gameId)
            end
        else
            print("Game not found! Use !game list to see available games.")
        end

    elseif action == "go" and L_arg0[2] then
        local gameId = tonumber(L_arg0[2])

        if gameId then
            print("Teleporting to game ID: " .. gameId)

            -- Attempt teleportation
            local success, err = pcall(function()
                game:GetService("TeleportService"):Teleport(gameId, L_4_)
            end)

            if success then
                game:GetService("StarterGui"):SetCore("ChatMakeSystemMessage", {
                    Text = "[GAMES] Teleporting...",
                    Color = Color3.fromRGB(0, 255, 255)
                })
            else
                print("Error teleporting: " .. err)
            end
        else
            print("Invalid Game ID! Use !game list to find valid IDs.")
        end
    else
        print("Invalid usage. Try !game list, !game copy [game_name], or !game go [game_id]")
    end
end,

["link"] = function(args)
    if #args < 1 then
        print("Usage: !link [list/copy] [link_name]")
        return
    end

    local action = args[1]:lower()

    if action == "list" then
        print("\n=== Available Links ===")
        for linkName, _ in pairs(links) do
            print(linkName)
        end
        print("========================\n")

        -- Send chat message with available links
        local linkNames = {}
        for linkName, _ in pairs(links) do
            table.insert(linkNames, linkName)
        end
        game:GetService("StarterGui"):SetCore("ChatMakeSystemMessage", {
            Text = "[LINKS] Available: " .. table.concat(linkNames, ", "),
            Color = Color3.fromRGB(0, 255, 255)
        })

    elseif action == "copy" and args[2] then
        local linkName = table.concat(args, " ", 2):lower()

        if links[linkName] then
            local link = links[linkName]

            if setclipboard then
                setclipboard(link)
                print("Copied Link: " .. link)

                game:GetService("StarterGui"):SetCore("ChatMakeSystemMessage", {
                    Text = "[LINKS] Link copied: " .. linkName,
                    Color = Color3.fromRGB(0, 255, 0)
                })
            else
                print("Clipboard function not available! Link: " .. link)
            end
        else
            print("Link not found! Use !link list to see available links.")
        end
    else
        print("Invalid usage. Try !link list or !link copy [link_name]")
    end
end,

["cf"] = function(args)
    local StarterGui = game:GetService("StarterGui")

    local message = "DOOM's chat filler"
    local numRepeats = 50

    -- Initial color (red)
    local red = 204
    local green = 0
    local blue = 0

    -- Target color (orange)
    local targetRed = 255
    local targetGreen = 102
    local targetBlue = 0

    -- Calculate color change per iteration
    local redChange = (targetRed - red) / numRepeats
    local greenChange = (targetGreen - green) / numRepeats
    local blueChange = (targetBlue - blue) / numRepeats

    for i = 1, numRepeats do
        -- Send the message
        StarterGui:SetCore("ChatMakeSystemMessage", {
            Text = message,
            Color = Color3.fromRGB(math.floor(red), math.floor(green), math.floor(blue))
        })

        -- Update the color components
        red = red + redChange
        green = green + greenChange
        blue = blue + blueChange

        -- Add a small delay
        wait(0.1)
    end
end,

["graph"] = function(args)
    local Lighting = game:GetService("Lighting")

    -- Clear any existing effects
    for _, item in pairs(Lighting:GetChildren()) do
        if item:IsA("PostEffect") or item:IsA("Sky") or item:IsA("Atmosphere") then
            item:Destroy()
        end
    end

    -- Lighting settings
    Lighting.Technology = Enum.Technology.Future
    Lighting.GlobalShadows = true
    Lighting.ShadowSoftness = 0.2
    Lighting.Brightness = 2.5
    Lighting.ExposureCompensation = 0.35
    Lighting.Ambient = Color3.fromRGB(35, 35, 40)
    Lighting.OutdoorAmbient = Color3.fromRGB(70, 70, 85)
    Lighting.GeographicLatitude = 45
    Lighting.EnvironmentDiffuseScale = 1
    Lighting.EnvironmentSpecularScale = 1

    -- Atmosphere
    local Atmosphere = Instance.new("Atmosphere", Lighting)
    Atmosphere.Density = 0.45
    Atmosphere.Offset = 0.35
    Atmosphere.Color = Color3.fromRGB(199, 205, 210)
    Atmosphere.Decay = Color3.fromRGB(90, 105, 120)
    Atmosphere.Glare = 0.45
    Atmosphere.Haze = 2.5

    -- Bloom Effect
    local Bloom = Instance.new("BloomEffect", Lighting)
    Bloom.Intensity = 0.65
    Bloom.Size = 32
    Bloom.Threshold = 1.5

    -- Depth of Field
    local DepthOfField = Instance.new("DepthOfFieldEffect", Lighting)
    DepthOfField.FarIntensity = 0.25
    DepthOfField.FocusDistance = 30
    DepthOfField.InFocusRadius = 45
    DepthOfField.NearIntensity = 0.5

    -- Sun Rays
    local SunRays = Instance.new("SunRaysEffect", Lighting)
    SunRays.Intensity = 0.35
    SunRays.Spread = 0.15

    -- Color Correction
    local ColorCorrection = Instance.new("ColorCorrectionEffect", Lighting)
    ColorCorrection.Brightness = 0.05
    ColorCorrection.Contrast = 0.15
    ColorCorrection.Saturation = 0.2
    ColorCorrection.TintColor = Color3.fromRGB(255, 252, 245)

    -- Blur Effect
    local Blur = Instance.new("BlurEffect", Lighting)
    Blur.Size = 1.5

    -- Enhanced Water
    local Terrain = workspace.Terrain
    Terrain.WaterWaveSize = 0.25
    Terrain.WaterWaveSpeed = 12
    Terrain.WaterReflectance = 0.85
    Terrain.WaterTransparency = 0.65

    -- HD Skybox
    local Sky = Instance.new("Sky", Lighting)
    Sky.SkyboxBk = "rbxassetid://8107841671"
    Sky.SkyboxDn = "rbxassetid://8107841765"
    Sky.SkyboxFt = "rbxassetid://8107841671"
    Sky.SkyboxLf = "rbxassetid://8107841671"
    Sky.SkyboxRt = "rbxassetid://8107841671"
    Sky.SkyboxUp = "rbxassetid://8107841776"
    Sky.SunAngularSize = 15
    Sky.MoonAngularSize = 25
    Sky.StarCount = 3000
    Sky.CelestialBodiesShown = true

    game:GetService("StarterGui"):SetCore("ChatMakeSystemMessage", {
        Text = "[GRAPHICS] Ultra graphics enabled! FPS may drop on low-end devices.",
        Color = Color3.fromRGB(0, 255, 255)
    })
end,

["ungraph"] = function(args)
    local Lighting = game:GetService("Lighting")
    local Terrain = workspace.Terrain

    -- Clear all graphics effects
    for _, effect in pairs(Lighting:GetChildren()) do
        if effect:IsA("PostEffect") or effect:IsA("Sky") or effect:IsA("Atmosphere") then
            effect:Destroy()
        end
    end

    -- Reset Lighting to default
    Lighting.Technology = Enum.Technology.Compatibility
    Lighting.GlobalShadows = false
    Lighting.Brightness = 1
    Lighting.Ambient = Color3.fromRGB(127, 127, 127)
    Lighting.OutdoorAmbient = Color3.fromRGB(127, 127, 127)
    Lighting.ClockTime = 14

    -- Reset Water
    Terrain.WaterWaveSize = 0
    Terrain.WaterWaveSpeed = 10
    Terrain.WaterReflectance = 0.5
    Terrain.WaterTransparency = 0.5

    game:GetService("StarterGui"):SetCore("ChatMakeSystemMessage", {
        Text = "[GRAPHICS] Ultra graphics disabled. Performance restored.",
        Color = Color3.fromRGB(255, 100, 100)
    })
end,

["graph2"] = function(args)
    local Lighting = game:GetService("Lighting")
    local Players = game:GetService("Players")
    local LocalPlayer = Players.LocalPlayer or Players.PlayerAdded:Wait()

    -- Clear any existing effects
    for _, item in pairs(Lighting:GetChildren()) do
        if item:IsA("PostEffect") or item:IsA("Sky") or item:IsA("Atmosphere") then
            item:Destroy()
        end
    end

    -- Apply Ultra Graphics (same as !graph)
    Lighting.Technology = Enum.Technology.Future
    Lighting.GlobalShadows = true
    Lighting.ShadowSoftness = 0.2
    Lighting.Brightness = 2.5
    Lighting.ExposureCompensation = 0.35
    Lighting.Ambient = Color3.fromRGB(35, 35, 40)
    Lighting.OutdoorAmbient = Color3.fromRGB(70, 70, 85)

    local Atmosphere = Instance.new("Atmosphere", Lighting)
    Atmosphere.Density = 0.45
    Atmosphere.Offset = 0.35
    Atmosphere.Color = Color3.fromRGB(199, 205, 210)
    Atmosphere.Decay = Color3.fromRGB(90, 105, 120)
    Atmosphere.Glare = 0.45
    Atmosphere.Haze = 2.5

    local Sky = Instance.new("Sky", Lighting)
    Sky.SkyboxBk = "rbxassetid://8107841671"
    Sky.SkyboxDn = "rbxassetid://8107841765"
    Sky.SkyboxFt = "rbxassetid://8107841671"
    Sky.SkyboxLf = "rbxassetid://8107841671"
    Sky.SkyboxRt = "rbxassetid://8107841671"
    Sky.SkyboxUp = "rbxassetid://8107841776"
    Sky.SunAngularSize = 15
    Sky.MoonAngularSize = 25
    Sky.StarCount = 3000
    Sky.CelestialBodiesShown = true

    -- GUI Toggle Button
    local ScreenGui = Instance.new("ScreenGui")
    ScreenGui.Name = "GraphicsGUI"
    ScreenGui.Parent = LocalPlayer:WaitForChild("PlayerGui")

    local ToggleButton = Instance.new("TextButton")
    ToggleButton.Name = "ToggleGraphics"
    ToggleButton.Text = "Disable Ultra Graphics"
    ToggleButton.Size = UDim2.new(0, 180, 0, 40)
    ToggleButton.Position = UDim2.new(0.9, -90, 0.05, 0)
    ToggleButton.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
    ToggleButton.TextColor3 = Color3.fromRGB(255, 255, 255)
    ToggleButton.Font = Enum.Font.SourceSansBold
    ToggleButton.TextSize = 16
    ToggleButton.Parent = ScreenGui

    -- Toggle Functionality
    local effectsEnabled = true
    ToggleButton.MouseButton1Click:Connect(function()
        if effectsEnabled then
            -- Disable Ultra Graphics
            for _, effect in pairs(Lighting:GetChildren()) do
                if effect:IsA("PostEffect") or effect:IsA("Sky") or effect:IsA("Atmosphere") then
                    effect:Destroy()
                end
            end
            Lighting.Technology = Enum.Technology.Compatibility
            Lighting.GlobalShadows = false
            Lighting.Brightness = 1
            Lighting.Ambient = Color3.fromRGB(127, 127, 127)
            Lighting.OutdoorAmbient = Color3.fromRGB(127, 127, 127)

            ToggleButton.Text = "Enable Ultra Graphics"
            effectsEnabled = false
        else
            -- Re-enable Ultra Graphics
            Lighting.Technology = Enum.Technology.Future
            Lighting.GlobalShadows = true
            Lighting.Brightness = 2.5
            Lighting.ExposureCompensation = 0.35

            local Atmosphere = Instance.new("Atmosphere", Lighting)
            Atmosphere.Density = 0.45
            Atmosphere.Offset = 0.35
            Atmosphere.Color = Color3.fromRGB(199, 205, 210)
            Atmosphere.Decay = Color3.fromRGB(90, 105, 120)
            Atmosphere.Glare = 0.45
            Atmosphere.Haze = 2.5

            local Sky = Instance.new("Sky", Lighting)
            Sky.SkyboxBk = "rbxassetid://8107841671"
            Sky.SkyboxDn = "rbxassetid://8107841765"
            Sky.SkyboxFt = "rbxassetid://8107841671"
            Sky.SkyboxLf = "rbxassetid://8107841671"
            Sky.SkyboxRt = "rbxassetid://8107841671"
            Sky.SkyboxUp = "rbxassetid://8107841776"
            Sky.SunAngularSize = 15
            Sky.MoonAngularSize = 25
            Sky.StarCount = 3000
            Sky.CelestialBodiesShown = true
            Sky.Parent = Lighting

            ToggleButton.Text = "Disable Ultra Graphics"
            effectsEnabled = true
        end
    end)

    game:GetService("StarterGui"):SetCore("ChatMakeSystemMessage", {
        Text = "[GRAPHICS] Ultra graphics enabled! Use GUI button to toggle.",
        Color = Color3.fromRGB(0, 255, 255)
    })
end,

["ungraph2"] = function(args)
    local Lighting = game:GetService("Lighting")
    local PlayerGui = game.Players.LocalPlayer:FindFirstChild("PlayerGui")

    -- Remove all effects
    for _, effect in pairs(Lighting:GetChildren()) do
        if effect:IsA("PostEffect") or effect:IsA("Sky") or effect:IsA("Atmosphere") then
            effect:Destroy()
        end
    end

    -- Reset Lighting to default
    Lighting.Technology = Enum.Technology.Compatibility
    Lighting.GlobalShadows = false
    Lighting.Brightness = 1
    Lighting.Ambient = Color3.fromRGB(127, 127, 127)
    Lighting.OutdoorAmbient = Color3.fromRGB(127, 127, 127)

    -- Remove GUI
    if PlayerGui then
        local Gui = PlayerGui:FindFirstChild("GraphicsGUI")
        if Gui then Gui:Destroy() end
    end

    game:GetService("StarterGui"):SetCore("ChatMakeSystemMessage", {
        Text = "[GRAPHICS] Ultra graphics disabled and GUI removed.",
        Color = Color3.fromRGB(255, 100, 100)
    })
end,

["trail"] = function(args)
    local player = game.Players.LocalPlayer
    if not player then
        warn("[ERROR] Player not found.")
        return
    end
    local character = player.Character or player.CharacterAdded:Wait()
    if not character then
        warn("[ERROR] Character not found.")
        return
    end
    local humanoidRootPart = character:FindFirstChild("HumanoidRootPart")
    if not humanoidRootPart then
        warn("[ERROR] HumanoidRootPart not found.")
        return
    end
    
    -- Check for color choice
    local colorChoice = args[1] and args[1]:lower() or "rainbow"
    if colorChoice == "list" then
        game:GetService("StarterGui"):SetCore("ChatMakeSystemMessage", {
            Text = "[TRIAL] Available colors: " .. table.concat(getKeys(Trails), ", ") .. " (add 'inf' for infinite trails)",
            Color = Color3.fromRGB(255, 255, 255)
        })
        return
    end
    
    -- Check for infinite option
    local isInfinite = args[2] and args[2]:lower() == "inf"
    local selectedColor = Trails[colorChoice] or Trails["rainbow"]
    
    -- Remove existing trails
    if _G.trailEffect then
        if type(_G.trailEffect) == "table" then
            for _, trail in ipairs(_G.trailEffect) do
                trail:Destroy()
            end
        else
            _G.trailEffect:Destroy()
        end
        _G.trailEffect = nil
    end
    
    if _G.trailAttachments then
        for _, att in ipairs(_G.trailAttachments) do
            att:Destroy()
        end
        _G.trailAttachments = nil
    end
    
    if _G.trailConnection then
        _G.trailConnection:Disconnect()
        _G.trailConnection = nil
    end
    
    -- Create square attachment configuration
    local attachments = {}
    local positions = {
        Vector3.new(-0.5, 0, -0.5),
        Vector3.new(0.5, 0, -0.5),
        Vector3.new(0.5, 0, 0.5),
        Vector3.new(-0.5, 0, 0.5)
    }
    
    for i = 1, 4 do
        local att = Instance.new("Attachment")
        att.Parent = humanoidRootPart
        att.Position = positions[i]
        table.insert(attachments, att)
    end
    
    -- Create trails between attachments
    local trails = {}
    for i = 1, 4 do
        local nextIndex = (i % 4) + 1
        local trail = Instance.new("Trail")
        trail.Parent = humanoidRootPart
        trail.Attachment0 = attachments[i]
        trail.Attachment1 = attachments[nextIndex]
        trail.FaceCamera = true
        
        -- Set infinite trail lifetime
        if isInfinite then
            trail.Lifetime = math.huge  -- **Infinite Lifetime**
            trail.Transparency = NumberSequence.new(0)  -- Always visible
        else
            trail.Lifetime = 1  -- Normal lifetime
            trail.Transparency = NumberSequence.new({
                NumberSequenceKeypoint.new(0, 0),    -- Start visible
                NumberSequenceKeypoint.new(0.7, 0),  -- Stay visible
                NumberSequenceKeypoint.new(1, 1)     -- Fade out
            })
        end
        
        trail.MinLength = 0.1
        trail.Enabled = true
        trail.LightInfluence = 0
        trail.WidthScale = NumberSequence.new(1)
        
        table.insert(trails, trail)
    end
    
    -- Store effects
    _G.trailEffect = trails
    _G.trailAttachments = attachments
    
    -- Apply colors
    if colorChoice == "rainbow" then
        _G.trailConnection = game:GetService("RunService").RenderStepped:Connect(function()
            local time = tick() % 5
            local rainbowColor = Color3.fromHSV(time / 5, 1, 1)
            for _, trail in ipairs(_G.trailEffect) do
                trail.Color = ColorSequence.new(rainbowColor)
            end
        end)
    else
        for _, trail in ipairs(_G.trailEffect) do
            trail.Color = ColorSequence.new(selectedColor)
        end
    end
    
    -- Send chat message
    local message = "[TRAIL] " .. colorChoice .. " trail"
    if isInfinite then
        message = message .. " (infinite)"
    end
    message = message .. " activated!"
    
    game:GetService("StarterGui"):SetCore("ChatMakeSystemMessage", {
        Text = message,
        Color = selectedColor
    })
end,

["untrail"] = function()
    if _G.trailEffect then
        if type(_G.trailEffect) == "table" then
            for _, trail in ipairs(_G.trailEffect) do
                trail:Destroy()
            end
        else
            _G.trailEffect:Destroy()
        end
        _G.trailEffect = nil
    end
    
    if _G.trailAttachments then
        for _, att in ipairs(_G.trailAttachments) do
            att:Destroy()
        end
        _G.trailAttachments = nil
    end
    
    if _G.trailConnection then
        _G.trailConnection:Disconnect()
        _G.trailConnection = nil
    end
    
    game:GetService("StarterGui"):SetCore("ChatMakeSystemMessage", {
        Text = "[TRAIL] Trail effect removed.",
        Color = Color3.fromRGB(255, 255, 255)
    })
end,

["setprefix"] = function(args)
    local newPrefix = args[1]
    
    -- Check if a valid prefix is given
    if not newPrefix or newPrefix == "" or #newPrefix > 3 then
        game:GetService("StarterGui"):SetCore("ChatMakeSystemMessage", {
            Text = "[PREFIX] Invalid prefix! Must be 1-3 characters.",
            Color = Color3.fromRGB(255, 0, 0)
        })
        return
    end
    
    -- Update global prefix
    _G.currentPrefix = newPrefix
    
    -- Confirmation message
    game:GetService("StarterGui"):SetCore("ChatMakeSystemMessage", {
        Text = "[PREFIX] Command prefix changed to: " .. newPrefix,
        Color = Color3.fromRGB(0, 255, 0)
    })
end,
}

-- Process chat messages
local function L_13_func(L_112_arg0)
	print("Message received: " .. L_112_arg0)
	if L_112_arg0:sub(1, 1) ~= _G.currentPrefix then
		return
	end
	local L_113_ = L_112_arg0:split(" ")
	local L_114_ = L_113_[1]:sub(2)
	local L_115_ = {}
	for L_116_forvar0 = 2, # L_113_ do
		table.insert(L_115_, L_113_[L_116_forvar0])
	end
	if L_12_[L_114_] then
		print("Executing command: " .. L_114_)
		L_12_[L_114_](L_115_)
	end
end

-- Connect chat handler
L_4_.Chatted:Connect(L_13_func)

-- Notify player that script is loaded
game:GetService("StarterGui"):SetCore("ChatMakeSystemMessage", {
	Text = "[COMMANDS LOADED] Available " .. L_11_,  -- Changed + to ..
	Color = Color3.fromRGB(0, 255, 0)
})

print("Chat commands initialized!")
