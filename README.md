-- ╔═══════════════════════════════════╗
--   EXO HUB | Infinite Jump
--   Delta Ready
-- ╚═══════════════════════════════════╝

local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")
local RunService = game:GetService("RunService")

local player = Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoid = character:WaitForChild("Humanoid")

local jumpEnabled = false

-- ╔══════════════════╗
--   GUI
-- ╚══════════════════╝

local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "ExoJumpGui"
ScreenGui.ResetOnSpawn = false
ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
ScreenGui.Parent = player.PlayerGui

local Hub = Instance.new("Frame")
Hub.Size = UDim2.new(0, 200, 0, 140)
Hub.Position = UDim2.new(1, -220, 0.5, -70)
Hub.BackgroundColor3 = Color3.fromRGB(6, 6, 14)
Hub.BorderSizePixel = 0
Hub.Parent = ScreenGui
Instance.new("UICorner", Hub).CornerRadius = UDim.new(0, 14)

local HubStroke = Instance.new("UIStroke")
HubStroke.Color = Color3.fromRGB(0, 200, 100)
HubStroke.Thickness = 1.5
HubStroke.Transparency = 0.4
HubStroke.Parent = Hub

local HubGrad = Instance.new("UIGradient")
HubGrad.Color = ColorSequence.new({
    ColorSequenceKeypoint.new(0, Color3.fromRGB(6, 18, 10)),
    ColorSequenceKeypoint.new(1, Color3.fromRGB(6, 6, 14)),
})
HubGrad.Rotation = 120
HubGrad.Parent = Hub

local TopBar = Instance.new("Frame")
TopBar.Size = UDim2.new(1, 0, 0, 3)
TopBar.BackgroundColor3 = Color3.fromRGB(0, 200, 100)
TopBar.BorderSizePixel = 0
TopBar.Parent = Hub

local TopBarGrad = Instance.new("UIGradient")
TopBarGrad.Color = ColorSequence.new({
    ColorSequenceKeypoint.new(0, Color3.fromRGB(0, 255, 120)),
    ColorSequenceKeypoint.new(0.5, Color3.fromRGB(0, 200, 255)),
    ColorSequenceKeypoint.new(1, Color3.fromRGB(100, 255, 0)),
})
TopBarGrad.Parent = TopBar

local Title = Instance.new("TextLabel")
Title.Size = UDim2.new(1, -36, 0, 36)
Title.Position = UDim2.new(0, 12, 0, 4)
Title.BackgroundTransparency = 1
Title.Text = "✦ EXO HUB"
Title.TextColor3 = Color3.fromRGB(255, 255, 255)
Title.TextSize = 16
Title.Font = Enum.Font.GothamBold
Title.TextXAlignment = Enum.TextXAlignment.Left
Title.Parent = Hub

local Sub = Instance.new("TextLabel")
Sub.Size = UDim2.new(1, -12, 0, 14)
Sub.Position = UDim2.new(0, 12, 0, 36)
Sub.BackgroundTransparency = 1
Sub.Text = "INFINITE JUMP"
Sub.TextColor3 = Color3.fromRGB(0, 200, 100)
Sub.TextSize = 9
Sub.Font = Enum.Font.GothamMedium
Sub.TextXAlignment = Enum.TextXAlignment.Left
Sub.Parent = Hub

local Div = Instance.new("Frame")
Div.Size = UDim2.new(1, -24, 0, 1)
Div.Position = UDim2.new(0, 12, 0, 58)
Div.BackgroundColor3 = Color3.fromRGB(0, 200, 100)
Div.BackgroundTransparency = 0.75
Div.BorderSizePixel = 0
Div.Parent = Hub

local CloseBtn = Instance.new("TextButton")
CloseBtn.Size = UDim2.new(0, 24, 0, 24)
CloseBtn.Position = UDim2.new(1, -32, 0, 8)
CloseBtn.BackgroundColor3 = Color3.fromRGB(255, 50, 80)
CloseBtn.BackgroundTransparency = 0.5
CloseBtn.Text = "✕"
CloseBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
CloseBtn.TextSize = 11
CloseBtn.Font = Enum.Font.GothamBold
CloseBtn.BorderSizePixel = 0
CloseBtn.Parent = Hub
Instance.new("UICorner", CloseBtn).CornerRadius = UDim.new(0, 6)

-- Toggle Row
local Row = Instance.new("Frame")
Row.Size = UDim2.new(1, -24, 0, 52)
Row.Position = UDim2.new(0, 12, 0, 68)
Row.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
Row.BackgroundTransparency = 0.97
Row.BorderSizePixel = 0
Row.Parent = Hub
Instance.new("UICorner", Row).CornerRadius = UDim.new(0, 10)

local RowStroke = Instance.new("UIStroke")
RowStroke.Color = Color3.fromRGB(0, 160, 80)
RowStroke.Thickness = 1
RowStroke.Transparency = 0.75
RowStroke.Parent = Row

local RowLabel = Instance.new("TextLabel")
RowLabel.Size = UDim2.new(1, -60, 0, 28)
RowLabel.Position = UDim2.new(0, 12, 0, 6)
RowLabel.BackgroundTransparency = 1
RowLabel.Text = "🦘  Infinite Jump"
RowLabel.TextColor3 = Color3.fromRGB(230, 255, 230)
RowLabel.TextSize = 13
RowLabel.Font = Enum.Font.GothamBold
RowLabel.TextXAlignment = Enum.TextXAlignment.Left
RowLabel.Parent = Row

local RowSub = Instance.new("TextLabel")
RowSub.Size = UDim2.new(1, -60, 0, 18)
RowSub.Position = UDim2.new(0, 12, 0, 30)
RowSub.BackgroundTransparency = 1
RowSub.Text = "Press SPACE to jump forever"
RowSub.TextColor3 = Color3.fromRGB(80, 160, 100)
RowSub.TextSize = 9
RowSub.Font = Enum.Font.Gotham
RowSub.TextXAlignment = Enum.TextXAlignment.Left
RowSub.Parent = Row

local PillBg = Instance.new("Frame")
PillBg.Size = UDim2.new(0, 40, 0, 22)
PillBg.Position = UDim2.new(1, -52, 0.5, -11)
PillBg.BackgroundColor3 = Color3.fromRGB(20, 40, 30)
PillBg.BorderSizePixel = 0
PillBg.Parent = Row
Instance.new("UICorner", PillBg).CornerRadius = UDim.new(1, 0)

local PillStroke = Instance.new("UIStroke")
PillStroke.Color = Color3.fromRGB(0, 120, 60)
PillStroke.Thickness = 1
PillStroke.Parent = PillBg

local Dot = Instance.new("Frame")
Dot.Size = UDim2.new(0, 16, 0, 16)
Dot.Position = UDim2.new(0, 3, 0.5, -8)
Dot.BackgroundColor3 = Color3.fromRGB(0, 120, 60)
Dot.BorderSizePixel = 0
Dot.Parent = PillBg
Instance.new("UICorner", Dot).CornerRadius = UDim.new(1, 0)

local ToggleBtn = Instance.new("TextButton")
ToggleBtn.Size = UDim2.new(1, 0, 1, 0)
ToggleBtn.BackgroundTransparency = 1
ToggleBtn.Text = ""
ToggleBtn.Parent = Row

-- Reopen
local ReopenBtn = Instance.new("TextButton")
ReopenBtn.Size = UDim2.new(0, 38, 0, 38)
ReopenBtn.Position = UDim2.new(1, -56, 0.5, -19)
ReopenBtn.BackgroundColor3 = Color3.fromRGB(6, 6, 14)
ReopenBtn.Text = "✦"
ReopenBtn.TextColor3 = Color3.fromRGB(0, 200, 100)
ReopenBtn.TextSize = 18
ReopenBtn.Font = Enum.Font.GothamBold
ReopenBtn.BorderSizePixel = 0
ReopenBtn.Visible = false
ReopenBtn.Parent = ScreenGui
Instance.new("UICorner", ReopenBtn).CornerRadius = UDim.new(0, 10)

local ReopenStroke = Instance.new("UIStroke")
ReopenStroke.Color = Color3.fromRGB(0, 200, 100)
ReopenStroke.Thickness = 1.5
ReopenStroke.Parent = ReopenBtn

-- ╔══════════════════╗
--   DRAGGING
-- ╚══════════════════╝

local function makeDraggable(frame)
    local dragging, dragStart, startPos = false, nil, nil
    frame.InputBegan:Connect(function(i)
        if i.UserInputType == Enum.UserInputType.MouseButton1 or i.UserInputType == Enum.UserInputType.Touch then
            dragging = true; dragStart = i.Position; startPos = frame.Position
        end
    end)
    frame.InputEnded:Connect(function(i)
        if i.UserInputType == Enum.UserInputType.MouseButton1 or i.UserInputType == Enum.UserInputType.Touch then
            dragging = false
        end
    end)
    UserInputService.InputChanged:Connect(function(i)
        if dragging and (i.UserInputType == Enum.UserInputType.MouseMovement or i.UserInputType == Enum.UserInputType.Touch) then
            local d = i.Position - dragStart
            frame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + d.X, startPos.Y.Scale, startPos.Y.Offset + d.Y)
        end
    end)
end

makeDraggable(Hub)
makeDraggable(ReopenBtn)

CloseBtn.MouseButton1Click:Connect(function()
    Hub.Visible = false
    ReopenBtn.Visible = true
end)

ReopenBtn.MouseButton1Click:Connect(function()
    Hub.Visible = true
    ReopenBtn.Visible = false
end)

-- ╔══════════════════╗
--   TOGGLE ANIMATION
-- ╚══════════════════╝

local function setToggle(state)
    if state then
        TweenService:Create(PillBg, TweenInfo.new(0.2), {BackgroundColor3 = Color3.fromRGB(0, 200, 80)}):Play()
        TweenService:Create(Dot, TweenInfo.new(0.2), {Position = UDim2.new(0, 21, 0.5, -8), BackgroundColor3 = Color3.fromRGB(255, 255, 255)}):Play()
        TweenService:Create(RowStroke, TweenInfo.new(0.2), {Transparency = 0.3, Color = Color3.fromRGB(0, 220, 100)}):Play()
    else
        TweenService:Create(PillBg, TweenInfo.new(0.2), {BackgroundColor3 = Color3.fromRGB(20, 40, 30)}):Play()
        TweenService:Create(Dot, TweenInfo.new(0.2), {Position = UDim2.new(0, 3, 0.5, -8), BackgroundColor3 = Color3.fromRGB(0, 120, 60)}):Play()
        TweenService:Create(RowStroke, TweenInfo.new(0.2), {Transparency = 0.75, Color = Color3.fromRGB(0, 160, 80)}):Play()
    end
end

-- ╔══════════════════╗
--   INFINITE JUMP
-- ╚══════════════════╝

UserInputService.JumpRequest:Connect(function()
    if jumpEnabled then
        local char = player.Character
        if char then
            local hum = char:FindFirstChild("Humanoid")
            if hum and hum:GetState() ~= Enum.HumanoidStateType.Dead then
                hum:ChangeState(Enum.HumanoidStateType.Jumping)
            end
        end
    end
end)

ToggleBtn.MouseButton1Click:Connect(function()
    jumpEnabled = not jumpEnabled
    setToggle(jumpEnabled)
end)

-- ╔══════════════════╗
--   IDLE ANIMATION
-- ╚══════════════════╝

local t = 0
RunService.Heartbeat:Connect(function(dt)
    t = t + dt
    local pulse = (math.sin(t * 2) + 1) / 2
    HubStroke.Transparency = 0.3 + pulse * 0.45
    TopBarGrad.Rotation = (t * 15) % 360
end)

-- ╔══════════════════╗
--   RESPAWN
-- ╚══════════════════╝

player.CharacterAdded:Connect(function(char)
    character = char
    humanoid = char:WaitForChild("Humanoid")
    jumpEnabled = false
    setToggle(false)
end)

print("[ExoHub] Infinite Jump Loaded ✓")
