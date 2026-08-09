# roblox-speed-boost-
a simple script ment to minulate getting the old speed boost glitches 

the script is fully for education purposes i do not recommend to use it i do not take any responiceablilty if something happens to your account
this is the script








--[[
    scripted by blix
]]
local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")
local RunService = game:GetService("RunService")
local CoreGui = game:GetService("CoreGui")
local UIS = game:GetService("UserInputService")
local player = Players.LocalPlayer
local guiName = "Blix_Speed_Boost"

if CoreGui:FindFirstChild(guiName) then
    CoreGui[guiName]:Destroy()
end

local screenGui = Instance.new("ScreenGui")
screenGui.Name = guiName
screenGui.ResetOnSpawn = false
screenGui.ZIndexBehavior = Enum.ZIndexBehavior.Global
screenGui.Parent = CoreGui

local running = true
local boostEnabled = true
local boostSpeed = 100
local MIN_SPEED = 32
local MAX_SPEED = 150
local SPEED_STEP = 5
local isBoosting = false
local minimized = false
local rainbowMode = false
local connections = {}

local function darken(c, amount)
    return Color3.new(
        math.clamp(c.R * amount, 0, 1),
        math.clamp(c.G * amount, 0, 1),
        math.clamp(c.B * amount, 0, 1)
    )
end

local baseColor = Color3.fromRGB(90, 40, 140)

local function getTheme(base)
    return {
        accent = base,
        window = darken(base, 0.32),
        title  = darken(base, 0.48),
        button = darken(base, 0.40),
        stroke = darken(base, 0.75),
        text   = Color3.fromRGB(235, 215, 255)
    }
end

local theme = getTheme(baseColor)

-- scripted by blix
local window = Instance.new("Frame")
window.Size = UDim2.new(0, 230, 0, 198)
window.Position = UDim2.new(0.5, -115, 0.55, 0)
window.BackgroundColor3 = theme.window
window.BorderSizePixel = 0
window.Active = true
window.Parent = screenGui

local windowCorner = Instance.new("UICorner")
windowCorner.CornerRadius = UDim.new(0, 12)
windowCorner.Parent = window

local windowStroke = Instance.new("UIStroke")
windowStroke.Color = theme.stroke
windowStroke.Thickness = 1.5
windowStroke.Parent = window

local titleBar = Instance.new("Frame")
titleBar.Size = UDim2.new(1, 0, 0, 30)
titleBar.BackgroundColor3 = theme.title
titleBar.BorderSizePixel = 0
titleBar.Parent = window

local titleCorner = Instance.new("UICorner")
titleCorner.CornerRadius = UDim.new(0, 12)
titleCorner.Parent = titleBar

local titleFix = Instance.new("Frame")
titleFix.Size = UDim2.new(1, 0, 0, 12)
titleFix.Position = UDim2.new(0, 0, 1, -12)
titleFix.BackgroundColor3 = theme.title
titleFix.BorderSizePixel = 0
titleFix.Parent = titleBar

local titleLabel = Instance.new("TextLabel")
titleLabel.Size = UDim2.new(1, -100, 1, 0)
titleLabel.Position = UDim2.new(0, 10, 0, 0)
titleLabel.BackgroundTransparency = 1
titleLabel.Text = "Blix's speed boost"
titleLabel.TextColor3 = theme.text
titleLabel.Font = Enum.Font.GothamBold
titleLabel.TextSize = 14
titleLabel.TextXAlignment = Enum.TextXAlignment.Left
titleLabel.Parent = titleBar

local settingsBtn = Instance.new("TextButton")
settingsBtn.Size = UDim2.new(0, 26, 0, 20)
settingsBtn.Position = UDim2.new(1, -88, 0.5, -10)
settingsBtn.BackgroundColor3 = darken(baseColor, 0.55)
settingsBtn.Text = "⚙"
settingsBtn.TextColor3 = theme.text
settingsBtn.Font = Enum.Font.GothamBold
settingsBtn.TextSize = 14
settingsBtn.AutoButtonColor = true
settingsBtn.Parent = titleBar

local settingsCorner = Instance.new("UICorner")
settingsCorner.CornerRadius = UDim.new(0, 6)
settingsCorner.Parent = settingsBtn

local minBtn = Instance.new("TextButton")
minBtn.Size = UDim2.new(0, 26, 0, 20)
minBtn.Position = UDim2.new(1, -58, 0.5, -10)
minBtn.BackgroundColor3 = darken(baseColor, 0.55)
minBtn.Text = "−"
minBtn.TextColor3 = theme.text
minBtn.Font = Enum.Font.GothamBold
minBtn.TextSize = 18
minBtn.AutoButtonColor = true
minBtn.Parent = titleBar

local minCorner = Instance.new("UICorner")
minCorner.CornerRadius = UDim.new(0, 6)
minCorner.Parent = minBtn

local closeBtn = Instance.new("TextButton")
closeBtn.Size = UDim2.new(0, 26, 0, 20)
closeBtn.Position = UDim2.new(1, -28, 0.5, -10)
closeBtn.BackgroundColor3 = Color3.fromRGB(110, 30, 55)
closeBtn.Text = "X"
closeBtn.TextColor3 = Color3.fromRGB(255, 190, 210)
closeBtn.Font = Enum.Font.GothamBold
closeBtn.TextSize = 14
closeBtn.AutoButtonColor = true
closeBtn.Parent = titleBar

local closeCorner = Instance.new("UICorner")
closeCorner.CornerRadius = UDim.new(0, 6)
closeCorner.Parent = closeBtn

local content = Instance.new("Frame")
content.Size = UDim2.new(1, 0, 1, -30)
content.Position = UDim2.new(0, 0, 0, 30)
content.BackgroundTransparency = 1
content.Parent = window

local statusFrame = Instance.new("Frame")
statusFrame.Size = UDim2.new(1, -12, 0, 48)
statusFrame.Position = UDim2.new(0, 6, 0, 8)
statusFrame.BackgroundColor3 = theme.button
statusFrame.BorderSizePixel = 0
statusFrame.Parent = content

local statusCorner = Instance.new("UICorner")
statusCorner.CornerRadius = UDim.new(0, 8)
statusCorner.Parent = statusFrame

local label = Instance.new("TextLabel")
label.Size = UDim2.new(1, 0, 1, 0)
label.BackgroundTransparency = 1
label.Text = "READY"
label.TextColor3 = theme.text
label.Font = Enum.Font.GothamBold
label.TextSize = 20
label.Parent = statusFrame

local multiFrame = Instance.new("Frame")
multiFrame.Size = UDim2.new(1, -12, 0, 44)
multiFrame.Position = UDim2.new(0, 6, 0, 64)
multiFrame.BackgroundColor3 = theme.button
multiFrame.BorderSizePixel = 0
multiFrame.Parent = content

local multiCorner = Instance.new("UICorner")
multiCorner.CornerRadius = UDim.new(0, 8)
multiCorner.Parent = multiFrame

local minusBtn = Instance.new("TextButton")
minusBtn.Size = UDim2.new(0, 44, 1, -8)
minusBtn.Position = UDim2.new(0, 6, 0, 4)
minusBtn.BackgroundColor3 = darken(baseColor, 0.55)
minusBtn.Text = "−"
minusBtn.TextColor3 = theme.text
minusBtn.Font = Enum.Font.GothamBold
minusBtn.TextSize = 22
minusBtn.AutoButtonColor = true
minusBtn.Parent = multiFrame

local minusCorner = Instance.new("UICorner")
minusCorner.CornerRadius = UDim.new(0, 6)
minusCorner.Parent = minusBtn

local speedLabel = Instance.new("TextLabel")
speedLabel.Size = UDim2.new(1, -108, 1, 0)
speedLabel.Position = UDim2.new(0, 54, 0, 0)
speedLabel.BackgroundTransparency = 1
speedLabel.Text = "100"
speedLabel.TextColor3 = theme.text
speedLabel.Font = Enum.Font.GothamBold
speedLabel.TextSize = 20
speedLabel.Parent = multiFrame

local plusBtn = Instance.new("TextButton")
plusBtn.Size = UDim2.new(0, 44, 1, -8)
plusBtn.Position = UDim2.new(1, -50, 0, 4)
plusBtn.BackgroundColor3 = darken(baseColor, 0.55)
plusBtn.Text = "+"
plusBtn.TextColor3 = theme.text
plusBtn.Font = Enum.Font.GothamBold
plusBtn.TextSize = 22
plusBtn.AutoButtonColor = true
plusBtn.Parent = multiFrame

local plusCorner = Instance.new("UICorner")
plusCorner.CornerRadius = UDim.new(0, 6)
plusCorner.Parent = plusBtn

local toggleBtn = Instance.new("TextButton")
toggleBtn.Size = UDim2.new(1, -12, 0, 44)
toggleBtn.Position = UDim2.new(0, 6, 0, 116)
toggleBtn.BackgroundColor3 = theme.accent
toggleBtn.Text = "ENABLE"
toggleBtn.TextColor3 = theme.text
toggleBtn.Font = Enum.Font.GothamBold
toggleBtn.TextSize = 16
toggleBtn.AutoButtonColor = true
toggleBtn.Parent = content

local toggleCorner = Instance.new("UICorner")
toggleCorner.CornerRadius = UDim.new(0, 8)
toggleCorner.Parent = toggleBtn

local colorWindow = Instance.new("Frame")
colorWindow.Size = UDim2.new(0, 448, 0, 200)
colorWindow.Position = UDim2.new(0.5, 130, 0.5, 0)
colorWindow.BackgroundColor3 = Color3.fromRGB(18, 18, 26)
colorWindow.BorderSizePixel = 0
colorWindow.Visible = false
colorWindow.Parent = screenGui

local colorCorner = Instance.new("UICorner")
colorCorner.CornerRadius = UDim.new(0, 10)
colorCorner.Parent = colorWindow

local colorStroke = Instance.new("UIStroke")
colorStroke.Color = theme.stroke
colorStroke.Thickness = 1.5
colorStroke.Parent = colorWindow

local colorTitle = Instance.new("TextLabel")
colorTitle.Size = UDim2.new(1, -90, 0, 28)
colorTitle.Position = UDim2.new(0, 12, 0, 4)
colorTitle.BackgroundTransparency = 1
colorTitle.Text = "Select Color"
colorTitle.TextColor3 = Color3.fromRGB(240, 240, 240)
colorTitle.Font = Enum.Font.GothamBold
colorTitle.TextSize = 16
colorTitle.TextXAlignment = Enum.TextXAlignment.Left
colorTitle.Parent = colorWindow

local colorCloseBtn = Instance.new("TextButton")
colorCloseBtn.Size = UDim2.new(0, 70, 0, 24)
colorCloseBtn.Position = UDim2.new(1, -80, 0, 6)
colorCloseBtn.BackgroundColor3 = Color3.fromRGB(45, 45, 60)
colorCloseBtn.Text = "Close"
colorCloseBtn.TextColor3 = Color3.fromRGB(240, 240, 240)
colorCloseBtn.Font = Enum.Font.GothamBold
colorCloseBtn.TextSize = 13
colorCloseBtn.AutoButtonColor = true
colorCloseBtn.Parent = colorWindow

local colorCloseCorner = Instance.new("UICorner")
colorCloseCorner.CornerRadius = UDim.new(0, 6)
colorCloseCorner.Parent = colorCloseBtn

local grid = Instance.new("Frame")
grid.Size = UDim2.new(1, -16, 0, 155)
grid.Position = UDim2.new(0, 8, 0, 36)
grid.BackgroundTransparency = 1
grid.Parent = colorWindow

local gridLayout = Instance.new("UIGridLayout")
gridLayout.CellSize = UDim2.new(0, 26, 0, 26)
gridLayout.CellPadding = UDim2.new(0, 3, 0, 3)
gridLayout.FillDirection = Enum.FillDirection.Horizontal
gridLayout.HorizontalAlignment = Enum.HorizontalAlignment.Left
gridLayout.VerticalAlignment = Enum.VerticalAlignment.Top
gridLayout.SortOrder = Enum.SortOrder.LayoutOrder
gridLayout.Parent = grid

local palette = {
    Color3.fromRGB(255, 40, 40),
    Color3.fromRGB(255, 90, 40),
    Color3.fromRGB(255, 150, 30),
    Color3.fromRGB(255, 220, 40),
    Color3.fromRGB(180, 255, 40),
    Color3.fromRGB(50, 255, 60),
    Color3.fromRGB(40, 255, 160),
    Color3.fromRGB(40, 220, 255),
    Color3.fromRGB(40, 140, 255),
    Color3.fromRGB(40, 60, 220),
    Color3.fromRGB(100, 50, 255),
    Color3.fromRGB(160, 40, 255),
    Color3.fromRGB(255, 40, 200),
    Color3.fromRGB(255, 40, 140),
    "RAINBOW",

    Color3.fromRGB(200, 25, 25),
    Color3.fromRGB(200, 70, 25),
    Color3.fromRGB(200, 110, 20),
    Color3.fromRGB(200, 170, 25),
    Color3.fromRGB(140, 200, 25),
    Color3.fromRGB(30, 190, 40),
    Color3.fromRGB(25, 190, 120),
    Color3.fromRGB(25, 170, 200),
    Color3.fromRGB(25, 100, 200),
    Color3.fromRGB(25, 40, 170),
    Color3.fromRGB(70, 30, 200),
    Color3.fromRGB(120, 25, 200),
    Color3.fromRGB(200, 25, 150),
    Color3.fromRGB(200, 25, 100),
    Color3.fromRGB(255, 255, 255),

    Color3.fromRGB(150, 20, 20),
    Color3.fromRGB(150, 55, 20),
    Color3.fromRGB(150, 85, 15),
    Color3.fromRGB(150, 130, 20),
    Color3.fromRGB(100, 150, 20),
    Color3.fromRGB(20, 140, 30),
    Color3.fromRGB(20, 140, 90),
    Color3.fromRGB(20, 120, 150),
    Color3.fromRGB(20, 70, 150),
    Color3.fromRGB(20, 30, 130),
    Color3.fromRGB(50, 20, 150),
    Color3.fromRGB(90, 20, 150),
    Color3.fromRGB(150, 20, 110),
    Color3.fromRGB(150, 20, 70),
    Color3.fromRGB(120, 120, 120),

    Color3.fromRGB(100, 12, 12),
    Color3.fromRGB(100, 35, 12),
    Color3.fromRGB(100, 55, 10),
    Color3.fromRGB(100, 85, 12),
    Color3.fromRGB(65, 100, 12),
    Color3.fromRGB(12, 90, 18),
    Color3.fromRGB(12, 90, 60),
    Color3.fromRGB(12, 75, 100),
    Color3.fromRGB(12, 45, 100),
    Color3.fromRGB(12, 18, 85),
    Color3.fromRGB(30, 12, 100),
    Color3.fromRGB(55, 12, 100),
    Color3.fromRGB(100, 12, 70),
    Color3.fromRGB(100, 12, 45),
    Color3.fromRGB(70, 70, 70),

    Color3.fromRGB(50, 6, 6),
    Color3.fromRGB(50, 18, 6),
    Color3.fromRGB(50, 28, 5),
    Color3.fromRGB(50, 42, 6),
    Color3.fromRGB(30, 50, 6),
    Color3.fromRGB(6, 45, 8),
    Color3.fromRGB(6, 45, 30),
    Color3.fromRGB(6, 38, 50),
    Color3.fromRGB(6, 22, 50),
    Color3.fromRGB(6, 8, 40),
    Color3.fromRGB(15, 6, 50),
    Color3.fromRGB(28, 6, 50),
    Color3.fromRGB(50, 6, 35),
    Color3.fromRGB(50, 6, 22),
    Color3.fromRGB(15, 15, 15),
}

local function applyTheme(base)
    baseColor = base
    theme = getTheme(base)

    window.BackgroundColor3 = theme.window
    titleBar.BackgroundColor3 = theme.title
    titleFix.BackgroundColor3 = theme.title
    windowStroke.Color = theme.stroke
    statusFrame.BackgroundColor3 = theme.button
    multiFrame.BackgroundColor3 = theme.button
    minusBtn.BackgroundColor3 = darken(base, 0.55)
    plusBtn.BackgroundColor3 = darken(base, 0.55)
    settingsBtn.BackgroundColor3 = darken(base, 0.55)
    minBtn.BackgroundColor3 = darken(base, 0.55)

    if boostEnabled then
        toggleBtn.BackgroundColor3 = theme.accent
    end

    titleLabel.TextColor3 = theme.text
    label.TextColor3 = theme.text
    speedLabel.TextColor3 = theme.text
    settingsBtn.TextColor3 = theme.text
    minBtn.TextColor3 = theme.text
    minusBtn.TextColor3 = theme.text
    plusBtn.TextColor3 = theme.text
    toggleBtn.TextColor3 = theme.text

    colorWindow.BackgroundColor3 = darken(base, 0.15)
    colorStroke.Color = theme.stroke
    colorCloseBtn.BackgroundColor3 = darken(base, 0.40)
end

local rainbowSwatch = nil

for i, entry in ipairs(palette) do
    local swatch = Instance.new("TextButton")
    swatch.LayoutOrder = i
    swatch.Text = ""
    swatch.AutoButtonColor = false
    swatch.Parent = grid

    local swatchCorner = Instance.new("UICorner")
    swatchCorner.CornerRadius = UDim.new(0, 4)
    swatchCorner.Parent = swatch

    if entry == "RAINBOW" then
        rainbowSwatch = swatch
        swatch.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
    else
        swatch.BackgroundColor3 = entry
    end

    swatch.MouseButton1Click:Connect(function()
        if not running then return end
        if entry == "RAINBOW" then
            rainbowMode = true
        else
            rainbowMode = false
            applyTheme(entry)
        end
    end)
end

-- scripted by blix
local hue = 0
table.insert(connections, RunService.Heartbeat:Connect(function(dt)
    if not running then return end

    hue = (hue + dt * 0.3) % 1
    local rainbow = Color3.fromHSV(hue, 1, 1)

    if rainbowSwatch then
        rainbowSwatch.BackgroundColor3 = rainbow
    end

    if rainbowMode then
        applyTheme(rainbow)
    end
end))

settingsBtn.MouseButton1Click:Connect(function()
    if not running or minimized then return end
    colorWindow.Visible = not colorWindow.Visible
end)

colorCloseBtn.MouseButton1Click:Connect(function()
    colorWindow.Visible = false
end)

local function updateSpeedDisplay()
    speedLabel.Text = tostring(boostSpeed)
end

minusBtn.MouseButton1Click:Connect(function()
    if not running or minimized then return end
    boostSpeed = math.clamp(boostSpeed - SPEED_STEP, MIN_SPEED, MAX_SPEED)
    updateSpeedDisplay()
end)

plusBtn.MouseButton1Click:Connect(function()
    if not running or minimized then return end
    boostSpeed = math.clamp(boostSpeed + SPEED_STEP, MIN_SPEED, MAX_SPEED)
    updateSpeedDisplay()
end)

toggleBtn.MouseButton1Click:Connect(function()
    if not running or minimized then return end
    boostEnabled = not boostEnabled
    if boostEnabled then
        toggleBtn.BackgroundColor3 = theme.accent
        toggleBtn.Text = "ENABLE"
        label.Text = "READY"
    else
        toggleBtn.BackgroundColor3 = Color3.fromRGB(100, 30, 55)
        toggleBtn.Text = "DISABLED"
        label.Text = "OFF"
        isBoosting = false
        local char = player.Character
        local hum = char and char:FindFirstChildOfClass("Humanoid")
        if hum then hum.WalkSpeed = 16 end
    end
end)

minBtn.MouseButton1Click:Connect(function()
    if not running then return end
    minimized = not minimized
    if minimized then
        content.Visible = false
        colorWindow.Visible = false
        TweenService:Create(window, TweenInfo.new(0.18), {Size = UDim2.new(0, 230, 0, 30)}):Play()
        minBtn.Text = "+"
    else
        content.Visible = true
        TweenService:Create(window, TweenInfo.new(0.18), {Size = UDim2.new(0, 230, 0, 198)}):Play()
        minBtn.Text = "−"
    end
end)

local function unload()
    running = false
    boostEnabled = false
    isBoosting = false
    rainbowMode = false
    local char = player.Character
    local hum = char and char:FindFirstChildOfClass("Humanoid")
    if hum then hum.WalkSpeed = 16 end
    for _, conn in ipairs(connections) do
        pcall(function() if conn.Connected then conn:Disconnect() end end)
    end
    if screenGui and screenGui.Parent then screenGui:Destroy() end
end

closeBtn.MouseButton1Click:Connect(unload)

table.insert(connections, RunService.Heartbeat:Connect(function()
    if not running then return end
    local char = player.Character
    local hum = char and char:FindFirstChildOfClass("Humanoid")
    if hum then
        local isActuallyJumping = hum.Jump or hum:GetState() == Enum.HumanoidStateType.Freefall or hum:GetState() == Enum.HumanoidStateType.Jumping
        if boostEnabled and isActuallyJumping then
            if not isBoosting then
                isBoosting = true
                hum.WalkSpeed = boostSpeed
                label.Text = "BOOSTING"
            else
                hum.WalkSpeed = boostSpeed
            end
        else
            if isBoosting then
                isBoosting = false
                hum.WalkSpeed = 16
                label.Text = boostEnabled and "READY" or "OFF"
            end
        end
    end
end))

local dragging, dragStart, startPos
table.insert(connections, titleBar.InputBegan:Connect(function(input)
    if not running then return end
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        dragging = true
        dragStart = input.Position
        startPos = window.Position
    end
end))

table.insert(connections, UIS.InputChanged:Connect(function(input)
    if not running then return end
    if dragging and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
        local delta = input.Position - dragStart
        window.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
    end
end))

table.insert(connections, UIS.InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        dragging = false
    end
end))

local colorDragging, colorDragStart, colorStartPos
colorWindow.InputBegan:Connect(function(input)
    if not running then return end
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        colorDragging = true
        colorDragStart = input.Position
        colorStartPos = colorWindow.Position
    end
end)

UIS.InputChanged:Connect(function(input)
    if not running then return end
    if colorDragging and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
        local delta = input.Position - colorDragStart
        colorWindow.Position = UDim2.new(colorStartPos.X.Scale, colorStartPos.X.Offset + delta.X, colorStartPos.Y.Scale, colorStartPos.Y.Offset + delta.Y)
    end
end)

UIS.InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        colorDragging = false
    end
end)

-- scripted by blix
