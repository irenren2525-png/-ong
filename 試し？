--========================================================--
--        PLATFORM SELECTION GUI (PC / Mobile)           --
--        ネオン風＋丸角＋半透明＋光るUI               --
--========================================================--

local Players = game:GetService("Players")
local player = Players.LocalPlayer
local CoreGui = game:GetService("CoreGui")

-- ScreenGui 作成
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "PlatformSelectionGUI"
ScreenGui.ResetOnSpawn = false
ScreenGui.Parent = CoreGui

-- メインフレーム
local Frame = Instance.new("Frame")
Frame.Size = UDim2.new(0, 320, 0, 160)
Frame.Position = UDim2.new(0.5, -160, 0.5, -80)
Frame.BackgroundColor3 = Color3.fromRGB(15,15,15)
Frame.BackgroundTransparency = 0.1
Frame.BorderSizePixel = 0
Frame.AnchorPoint = Vector2.new(0.5,0.5)
Frame.Parent = ScreenGui

-- 丸角
local frameCorner = Instance.new("UICorner")
frameCorner.CornerRadius = UDim.new(0,20)
frameCorner.Parent = Frame

-- フレームの光る枠線
local frameStroke = Instance.new("UIStroke")
frameStroke.Thickness = 3
frameStroke.Color = Color3.fromRGB(0,255,255)
frameStroke.Transparency = 0.3
frameStroke.Parent = Frame

-- タイトル
local Title = Instance.new("TextLabel")
Title.Size = UDim2.new(1,0,0,40)
Title.Position = UDim2.new(0,0,0,0)
Title.BackgroundTransparency = 1
Title.Text = "プラットフォーム選択"
Title.TextColor3 = Color3.fromRGB(0,255,255)
Title.Font = Enum.Font.GothamBold
Title.TextScaled = true
Title.Parent = Frame

-- ボタン作成関数
local function CreateButton(parent, text, posX, color)
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(0, 140, 0, 50)
    btn.Position = UDim2.new(posX, 0, 0.5, -25)
    btn.Text = text
    btn.TextScaled = true
    btn.Font = Enum.Font.GothamBold
    btn.TextColor3 = Color3.fromRGB(0,0,0)
    btn.BackgroundColor3 = color
    btn.BorderSizePixel = 0
    btn.AnchorPoint = Vector2.new(0,0.5)
    
    -- 丸角
    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0,15)
    corner.Parent = btn

    -- 光る枠線
    local stroke = Instance.new("UIStroke")
    stroke.Thickness = 2
    stroke.Color = Color3.fromRGB(0,255,255)
    stroke.Transparency = 0.2
    stroke.Parent = btn

    btn.Parent = parent
    return btn
end

-- PC ボタン
local PCButton = CreateButton(Frame, "PC", 0.05, Color3.fromRGB(0,200,255))
-- Mobile ボタン
local MobileButton = CreateButton(Frame, "IOSまたはAndroid(停止中)", 0.5, Color3.fromRGB(255,0,255))

--================= ボタンクリック処理 =================--
PCButton.MouseButton1Click:Connect(function()
    Frame:Destroy()
    loadstring(game:HttpGet("https://raw.githubusercontent.com/huroppar/Cheat/refs/heads/main/Xeno.lua"))()
end)

MobileButton.MouseButton1Click:Connect(function()
    Frame:Destroy()
   loadstring(game:HttpGet("https://raw.githubusercontent.com/huroppar/Cheat/refs/heads/main/Delta.lua"))()
end)
