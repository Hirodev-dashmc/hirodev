-- GUI Builder Script: Creates the GenerateGui programmatically
-- Enhanced with selection, inspection, copy, and highlight features.

local UserInputService = game:GetService("UserInputService")
local GuiService = game:GetService("GuiService")
local TweenService = game:GetService("TweenService")
local Players = game:GetService("Players")

local player = Players.LocalPlayer
local playerGui = nil

if player then
	playerGui = player:WaitForChild("PlayerGui", 5)
end

-- Fallback polling if player/PlayerGui is not immediately available
if not playerGui then
	local start = os.clock()
	while os.clock() - start < 5 do
		player = Players.LocalPlayer
		if player then
			playerGui = player:FindFirstChild("PlayerGui")
			if playerGui then break end
		end
		task.wait(0.1)
	end
end

local coreGuiSuccess, coreGui = pcall(function() return game:GetService("CoreGui") end)

local function buildGUI()
	-- Determine target parent container dynamically
	local targetParent
	if coreGuiSuccess and coreGui then
		-- Check if CoreGui is writable in this environment
		local success = pcall(function()
			local test = Instance.new("Folder")
			test.Parent = coreGui
			test:Destroy()
		end)
		if success then
			targetParent = coreGui
		end
	end

	if not targetParent then
		targetParent = playerGui
	end

	if not targetParent and script then
		targetParent = script.Parent
	end

	-- Destroy existing GUI from targetParent to avoid duplicates
	if targetParent then
		local existing = targetParent:FindFirstChild("GenerateGui")
		if existing then
			existing:Destroy()
		end
	end

	-- Also clean up other possible locations to be thorough
	if coreGuiSuccess and coreGui and coreGui ~= targetParent then
		pcall(function()
			local existing = coreGui:FindFirstChild("GenerateGui")
			if existing then existing:Destroy() end
		end)
	end
	if playerGui and playerGui ~= targetParent then
		pcall(function()
			local existing = playerGui:FindFirstChild("GenerateGui")
			if existing then existing:Destroy() end
		end)
	end

	-- ScreenGui
	local screenGui = Instance.new("ScreenGui")
	screenGui.Name = "GenerateGui"
	screenGui.ResetOnSpawn = true
	screenGui.ScreenInsets = Enum.ScreenInsets.CoreUISafeInsets
	screenGui.SafeAreaCompatibility = Enum.SafeAreaCompatibility.FullscreenExtension
	screenGui.Parent = targetParent

	-- GenerateFrame
	local generateFrame = Instance.new("Frame")
	generateFrame.Name = "GenerateFrame"
	generateFrame.Size = UDim2.new(0, 419, 0, 255)
	generateFrame.Position = UDim2.new(0.312, 0, 0.419, 0)
	generateFrame.BackgroundColor3 = Color3.fromRGB(53, 53, 53)
	generateFrame.BackgroundTransparency = 0.4
	generateFrame.BorderSizePixel = 0
	generateFrame.Parent = screenGui

	-- CodeBox (TextBox)
	local codeBox = Instance.new("TextBox")
	codeBox.Name = "CodeBox"
	codeBox.Size = UDim2.new(0, 202, 0, 99)
	codeBox.Position = UDim2.new(0.273, 0, 0.237, 0)
	codeBox.BackgroundColor3 = Color3.fromRGB(24, 24, 24)
	codeBox.BackgroundTransparency = 0.6
	codeBox.BorderSizePixel = 0
	codeBox.Text = ""
	codeBox.TextColor3 = Color3.fromRGB(255, 255, 255) -- High contrast white text
	codeBox.TextSize = 12 -- Slightly smaller text to fit more info
	codeBox.FontFace = Font.new("rbxasset://fonts/families/SourceSansPro.json", Enum.FontWeight.Regular, Enum.FontStyle.Normal)
	codeBox.PlaceholderColor3 = Color3.fromRGB(128, 128, 128)
	codeBox.PlaceholderText = "Click Generate to inspect a GUI object..."
	codeBox.ClearTextOnFocus = false -- Disable clearing on focus so player can select text
	codeBox.ShowNativeInput = true
	codeBox.TextXAlignment = Enum.TextXAlignment.Left -- Left alignment for readability
	codeBox.TextYAlignment = Enum.TextYAlignment.Top -- Top alignment for reading lines
	codeBox.MultiLine = true -- Enable multiple lines
	codeBox.TextWrapped = true -- Enable wrapping
	codeBox.Parent = generateFrame

	-- Copy Button
	local copyButton = Instance.new("TextButton")
	copyButton.Name = "Copy"
	copyButton.Size = UDim2.new(0, 200, 0, 50)
	copyButton.Position = UDim2.new(0.276, 0, 0.709, 0)
	copyButton.BackgroundColor3 = Color3.fromRGB(85, 255, 0)
	copyButton.BorderSizePixel = 0
	copyButton.Text = "Copy"
	copyButton.TextColor3 = Color3.fromRGB(0, 0, 0)
	copyButton.TextSize = 14
	copyButton.FontFace = Font.new("rbxasset://fonts/families/LuckiestGuy.json", Enum.FontWeight.Regular, Enum.FontStyle.Normal)
	copyButton.AutoButtonColor = true
	copyButton.Parent = generateFrame

	-- Close Button
	local closeButton = Instance.new("TextButton")
	closeButton.Name = "Close"
	closeButton.Size = UDim2.new(0, 65, 0, 31)
	closeButton.Position = UDim2.new(0.819, 0, 0.051, 0)
	closeButton.BackgroundColor3 = Color3.fromRGB(89, 89, 89)
	closeButton.BorderSizePixel = 0
	closeButton.Text = "Close"
	closeButton.TextColor3 = Color3.fromRGB(0, 0, 0)
	closeButton.TextSize = 14
	closeButton.FontFace = Font.new("rbxasset://fonts/families/FredokaOne.json", Enum.FontWeight.Regular, Enum.FontStyle.Normal)
	closeButton.AutoButtonColor = true
	closeButton.Parent = generateFrame

	-- Generate Button
	local generateButton = Instance.new("TextButton")
	generateButton.Name = "GenerateButton"
	generateButton.Size = UDim2.new(0, 138, 0, 50)
	generateButton.Position = UDim2.new(0.341, 0, 0.012, 0)
	generateButton.BackgroundColor3 = Color3.fromRGB(0, 170, 0)
	generateButton.BorderSizePixel = 0
	generateButton.Text = "Generate"
	generateButton.TextColor3 = Color3.fromRGB(0, 0, 0)
	generateButton.TextSize = 14
	generateButton.FontFace = Font.new("rbxasset://fonts/families/PressStart2P.json", Enum.FontWeight.Regular, Enum.FontStyle.Normal)
	generateButton.AutoButtonColor = true
	generateButton.Parent = generateFrame

	-- Open Button (visible when GUI is closed)
	local openButton = Instance.new("TextButton")
	openButton.Name = "OpenButton"
	openButton.Size = UDim2.new(0, 100, 0, 40)
	openButton.Position = UDim2.new(0, 10, 0.5, -20)
	openButton.BackgroundColor3 = Color3.fromRGB(0, 170, 0)
	openButton.BorderSizePixel = 0
	openButton.Text = "Open"
	openButton.TextColor3 = Color3.fromRGB(255, 255, 255)
	openButton.TextSize = 14
	openButton.FontFace = Font.new("rbxasset://fonts/families/SourceSansPro.json", Enum.FontWeight.Bold, Enum.FontStyle.Normal)
	openButton.AutoButtonColor = true
	openButton.Visible = false
	openButton.Parent = screenGui

	-- Selection Mode State
	local isSelecting = false

	-- Helper function to generate full Roblox object path
	local function getFullPath(obj)
		local parts = {}
		local current = obj
		while current and current ~= game do
			table.insert(parts, 1, current.Name)
			current = current.Parent
		end
		return "game." .. table.concat(parts, ".")
	end

	-- Helper function to format float values cleanly
	local function formatFloat(val)
		if val == math.floor(val) then
			return tostring(val)
		end
		local s = string.format("%.3f", val)
		s = s:gsub("%.?0+$", "") -- remove trailing zeros
		s = s:gsub("%.$", "")    -- remove trailing dot
		return s
	end

	-- Helper function to format UDim2 cleanly
	local function formatUDim2(udim2)
		return string.format("UDim2.new(%s, %d, %s, %d)", 
			formatFloat(udim2.X.Scale), udim2.X.Offset, 
			formatFloat(udim2.Y.Scale), udim2.Y.Offset)
	end

	-- Helper function to format Vector2 cleanly
	local function formatVector2(v2)
		return string.format("Vector2.new(%s, %s)", formatFloat(v2.X), formatFloat(v2.Y))
	end

	-- Helper function to format Color3 cleanly
	local function formatColor3(color)
		local r = math.round(color.R * 255)
		local g = math.round(color.G * 255)
		local b = math.round(color.B * 255)
		return string.format("Color3.fromRGB(%d, %d, %d)", r, g, b)
	end

	-- Highlight the selected object briefly
	local function highlightObject(obj)
		local highlight = Instance.new("Frame")
		highlight.Name = "SelectionHighlight"
		highlight.Size = UDim2.new(1, 0, 1, 0)
		highlight.Position = UDim2.new(0, 0, 0, 0)
		highlight.BackgroundColor3 = Color3.fromRGB(0, 170, 255)
		highlight.BackgroundTransparency = 0.5
		highlight.BorderSizePixel = 0
		highlight.ZIndex = 10000 -- Display on top of the elements
		
		local uiStroke = Instance.new("UIStroke")
		uiStroke.Color = Color3.fromRGB(255, 255, 255)
		uiStroke.Thickness = 3
		uiStroke.Transparency = 0
		uiStroke.Parent = highlight
		
		highlight.Parent = obj
		
		-- Animate fading out over 0.6 seconds
		local tweenInfo = TweenInfo.new(0.6, Enum.EasingStyle.Quad, Enum.EasingDirection.Out)
		local tweenBg = TweenService:Create(highlight, tweenInfo, {
			BackgroundTransparency = 1
		})
		local tweenStroke = TweenService:Create(uiStroke, tweenInfo, {
			Transparency = 1
		})
		
		tweenBg:Play()
		tweenStroke:Play()
		
		task.spawn(function()
			task.wait(0.6)
			highlight:Destroy()
		end)
	end

	-- Extract and display information about the clicked GUI object
	local function analyzeObject(obj)
		local lines = {}
		table.insert(lines, string.format("Instance Name: %s", obj.Name))
		table.insert(lines, string.format("ClassName: %s", obj.ClassName))
		
		-- Visible Text (if applicable)
		local hasText = false
		local successText, text = pcall(function() return obj.Text end)
		if successText and type(text) == "string" then
			table.insert(lines, string.format("Visible Text: %s", text))
			hasText = true
		end
		
		table.insert(lines, string.format("Full Path: %s", getFullPath(obj)))
		table.insert(lines, string.format("Position: %s", formatUDim2(obj.Position)))
		table.insert(lines, string.format("Size: %s", formatUDim2(obj.Size)))
		table.insert(lines, string.format("AnchorPoint: %s", formatVector2(obj.AnchorPoint)))
		table.insert(lines, string.format("BackgroundColor3: %s", formatColor3(obj.BackgroundColor3)))
		
		-- TextColor3 (if applicable)
		if hasText then
			local successColor, textColor = pcall(function() return obj.TextColor3 end)
			if successColor and typeof(textColor) == "Color3" then
				table.insert(lines, string.format("TextColor3: %s", formatColor3(textColor)))
			end
		end
		
		table.insert(lines, string.format("BorderSizePixel: %d", obj.BorderSizePixel))
		table.insert(lines, string.format("BackgroundTransparency: %s", formatFloat(obj.BackgroundTransparency)))
		table.insert(lines, string.format("Rotation: %s", formatFloat(obj.Rotation)))
		table.insert(lines, string.format("Visible: %s", tostring(obj.Visible)))
		table.insert(lines, string.format("ZIndex: %d", obj.ZIndex))
		table.insert(lines, string.format("Parent Name: %s", obj.Parent and obj.Parent.Name or "nil"))
		
		codeBox.Text = table.concat(lines, "\n")
		
		-- Highlight object
		highlightObject(obj)
	end

	-- Connect generateButton click to toggle selection mode
	generateButton.MouseButton1Click:Connect(function()
		if isSelecting then
			-- Cancel selection mode
			isSelecting = false
			generateButton.Text = "Generate"
			generateButton.BackgroundColor3 = Color3.fromRGB(0, 170, 0)
		else
			-- Enter selection mode
			isSelecting = true
			generateButton.Text = "Selecting..."
			generateButton.BackgroundColor3 = Color3.fromRGB(255, 170, 0) -- Orange color to signify active state
		end
	end)

	-- Handle clicks anywhere on the screen during selection mode
	local inputConnection
	inputConnection = UserInputService.InputBegan:Connect(function(input, gameProcessed)
		if not isSelecting then return end
		
		if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
			local mousePos = UserInputService:GetMouseLocation()
			local inset = GuiService:GetGuiInset()
			local adjustedPos = mousePos - inset
			
			local objects = {}
			if playerGui then
				pcall(function()
					local pgObjects = playerGui:GetGuiObjectsAtPosition(adjustedPos.X, adjustedPos.Y)
					for _, obj in ipairs(pgObjects) do
						table.insert(objects, obj)
					end
				end)
			end
			if coreGuiSuccess and coreGui then
				pcall(function()
					local cgObjects = coreGui:GetGuiObjectsAtPosition(adjustedPos.X, adjustedPos.Y)
					for _, obj in ipairs(cgObjects) do
						table.insert(objects, obj)
					end
				end)
			end
			local selectedObject = nil
			
			for _, obj in ipairs(objects) do
				-- Ignore objects belonging to our own inspector GUI
				if not obj:IsDescendantOf(screenGui) then
					local className = obj.ClassName
					-- Supported class check
					if className == "TextButton" or className == "ImageButton" or className == "TextLabel" or 
					   className == "Frame" or className == "ImageLabel" or className == "ScrollingFrame" or 
					   className == "TextBox" then
						selectedObject = obj
						break
					end
				end
			end
			
			if selectedObject then
				-- Automatically exit selection mode
				isSelecting = false
				generateButton.Text = "Generate"
				generateButton.BackgroundColor3 = Color3.fromRGB(0, 170, 0)
				
				-- Analyze the object
				analyzeObject(selectedObject)
			end
		end
	end)

	-- Clean up connection when ScreenGui is destroyed
	screenGui.Destroying:Connect(function()
		if inputConnection then
			inputConnection:Disconnect()
		end
	end)

	-- Button functionality
	closeButton.MouseButton1Click:Connect(function()
		generateFrame.Visible = false
		openButton.Visible = true
	end)

	openButton.MouseButton1Click:Connect(function()
		generateFrame.Visible = true
		openButton.Visible = false
	end)

	copyButton.MouseButton1Click:Connect(function()
		if codeBox.Text ~= "" then
			local success, err = pcall(function()
				setclipboard(codeBox.Text)
			end)
			if success then
				local originalText = copyButton.Text
				copyButton.Text = "Copied!"
				copyButton.BackgroundColor3 = Color3.fromRGB(0, 200, 0)
				task.delay(1, function()
					copyButton.Text = originalText
					copyButton.BackgroundColor3 = Color3.fromRGB(85, 255, 0)
				end)
			else
				-- Feedback for environments that don't support setclipboard directly
				local originalText = copyButton.Text
				copyButton.Text = "Failed to copy!"
				copyButton.BackgroundColor3 = Color3.fromRGB(200, 0, 0)
				copyButton.TextColor3 = Color3.fromRGB(255, 255, 255)
				task.delay(1.5, function()
					copyButton.Text = originalText
					copyButton.BackgroundColor3 = Color3.fromRGB(85, 255, 0)
					copyButton.TextColor3 = Color3.fromRGB(0, 0, 0)
				end)
			end
		end
	end)

	return screenGui
end

return buildGUI()
