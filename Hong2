local Rayfield = loadstring(game:HttpGet("https://sirius.menu/rayfield"))()
--━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
-- Services
--━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local Lighting = game:GetService("Lighting")
local Workspace = game:GetService("Workspace")
local HttpService = game:GetService("HttpService")
local TeleportService = game:GetService("TeleportService")
local GuiService = game:GetService("GuiService")
local Stats = game:GetService("Stats")
local LocalPlayer = Players.LocalPlayer
local Camera = Workspace.CurrentCamera
--━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
-- Character Utilities
--━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
local function getCharacter()
    local char = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()
    local hum = char:FindFirstChildOfClass("Humanoid")
    local hrp = char:FindFirstChild("HumanoidRootPart")
    return char, hum, hrp
end
local function getRoot()
    local _, _, root = getCharacter()
    return root
end
--━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
-- Window Creation
--━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
local Window = Rayfield:CreateWindow({
    Name = "Furo Hub",
    LoadingTitle = "読み込み中...",
    LoadingSubtitle = "Edited by Furopper",
    ConfigurationSaving = {
        Enabled = true,
        FolderName = "FuroHub",
        FileName = "Player"
    },
    KeySystem = false
})
--━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
-- Tab: プレイヤー
--━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
--━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
-- Tab: プレイヤー（完全版）
--━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
local PlayerTab = Window:CreateTab("プレイヤー", 4483362458)
-- ── 共通ユーティリティ（タブ内でも再利用） ──
local function getCharacter()
    local char = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()
    return char, char:FindFirstChildOfClass("Humanoid"), char:FindFirstChild("HumanoidRootPart")
end
-- ── スピード ────────────────────────────────────────
local speedEnabled = false
local speedValue = 30
local originalWalkSpeed
PlayerTab:CreateToggle({
    Name = "スピード",
    CurrentValue = false,
    Callback = function(enabled)
        speedEnabled = enabled
        local _, hum = getCharacter()
        if not hum then return end
        if enabled then
            originalWalkSpeed = hum.WalkSpeed
            hum.WalkSpeed = speedValue
        else
            hum.WalkSpeed = originalWalkSpeed or 16
        end
    end,
})
PlayerTab:CreateSlider({
    Name = "スピード値",
    Range = {16, 500},
    Increment = 1,
    Suffix = " studs/s",
    CurrentValue = speedValue,
    Callback = function(value)
        speedValue = value
        if not speedEnabled then return end
        local _, hum = getCharacter()
        if hum then hum.WalkSpeed = value end
    end,
})
-- ── ジャンプ力 ──────────────────────────────────────
local jumpEnabled = false
local jumpPowerValue = 50
local originalJumpPower
PlayerTab:CreateToggle({
    Name = "ジャンプ力変更",
    CurrentValue = false,
    Callback = function(enabled)
        jumpEnabled = enabled
        local _, hum = getCharacter()
        if not hum then return end
        if enabled then
            originalJumpPower = hum.JumpPower
            hum.JumpPower = jumpPowerValue
        else
            hum.JumpPower = originalJumpPower or 50
        end
    end,
})
PlayerTab:CreateSlider({
    Name = "ジャンプ力値",
    Range = {50, 700},
    Increment = 5,
    Suffix = "",
    CurrentValue = jumpPowerValue,
    Callback = function(value)
        jumpPowerValue = value
        if not jumpEnabled then return end
        local _, hum = getCharacter()
        if hum then hum.JumpPower = value end
    end,
})
-- ── 無限ジャンプ ────────────────────────────────────
local infiniteJumpEnabled = false
PlayerTab:CreateToggle({
    Name = "無限ジャンプ",
    CurrentValue = false,
    Callback = function(v) infiniteJumpEnabled = v end,
})
UserInputService.JumpRequest:Connect(function()
    if not infiniteJumpEnabled then return end
    local _, hum = getCharacter()
    if hum then
        hum:ChangeState(Enum.HumanoidStateType.Jumping)
    end
end)
-- ── 壁抜け（Noclip） ────────────────────────────────
local noclipEnabled = false
local noclipConn
local originalCanCollideCache = {}
local function enableNoclip()
    if noclipConn then return end
    local char = LocalPlayer.Character
    if not char then return end
    originalCanCollideCache = {}
    for _, part in ipairs(char:GetDescendants()) do
        if part:IsA("BasePart") then
            originalCanCollideCache[part] = part.CanCollide
        end
    end
    noclipConn = RunService.Stepped:Connect(function()
        local char = LocalPlayer.Character
        if not char then return end
        for _, part in ipairs(char:GetDescendants()) do
            if part:IsA("BasePart") then
                part.CanCollide = false
            end
        end
    end)
end
local function disableNoclip()
    if noclipConn then
        noclipConn:Disconnect()
        noclipConn = nil
    end
    local char = LocalPlayer.Character
    if not char then return end
    for part, can in pairs(originalCanCollideCache) do
        if part and part.Parent then
            part.CanCollide = can
        end
    end
    table.clear(originalCanCollideCache)
end
PlayerTab:CreateToggle({
    Name = "壁抜け (Noclip)",
    CurrentValue = false,
    Callback = function(v)
        noclipEnabled = v
        if v then
            enableNoclip()
        else
            disableNoclip()
        end
    end,
})
-- ── 空中テレポート（高所⇔地上往復） ─────────────────
local airTPActive = false
local airHeight = 2000
local airOriginCFrame
local airBodyVelocity
PlayerTab:CreateButton({
    Name = "空中TP (2000 stud)",
    Callback = function()
        local _, _, hrp = getCharacter()
        if not hrp then return end
        if not airTPActive then
            -- 上へ
            airOriginCFrame = hrp.CFrame
            hrp.CFrame = hrp.CFrame + Vector3.new(0, airHeight, 0)
            airBodyVelocity = Instance.new("BodyVelocity")
            airBodyVelocity.MaxForce = Vector3.new(0, math.huge, 0)
            airBodyVelocity.Velocity = Vector3.new()
            airBodyVelocity.Parent = hrp
            airTPActive = true
        else
            -- 元に戻す
            if airBodyVelocity then
                airBodyVelocity:Destroy()
                airBodyVelocity = nil
            end
            if airOriginCFrame then
                hrp.CFrame = airOriginCFrame
            end
            airTPActive = false
        end
    end,
})
-- ── 簡易足場 ────────────────────────────────────────
local createdPlatforms = {}
PlayerTab:CreateButton({
    Name = "足場を生成 (現在位置下)",
    Callback = function()
        local _, _, root = getCharacter()
        if not root then return end
        local part = Instance.new("Part")
        part.Size = Vector3.new(6, 1, 6)
        part.Anchored = true
        part.CanCollide = true
        part.Color = Color3.fromRGB(255, 200, 0)
        part.Material = Enum.Material.Neon
        part.CFrame = root.CFrame * CFrame.new(0, -3.1, 0)
        part.Parent = Workspace
        table.insert(createdPlatforms, part)
    end,
})
PlayerTab:CreateButton({
    Name = "生成した足場を全削除",
    Callback = function()
        for _, p in ipairs(createdPlatforms) do
            p:Destroy()
        end
        table.clear(createdPlatforms)
    end,
})
-- ── 位置固定（フリーズ） ─────────────────────────────
local freezeEnabled = false
local freezeConnection
local freezeCFrame
PlayerTab:CreateToggle({
    Name = "位置固定 (Freeze)",
    CurrentValue = false,
    Callback = function(v)
        freezeEnabled = v
        local _, _, hrp = getCharacter()
        if not hrp then return end
        if v then
            freezeCFrame = hrp.CFrame
            freezeConnection = RunService.RenderStepped:Connect(function()
                if hrp then
                    hrp.CFrame = freezeCFrame
                end
            end)
        else
            if freezeConnection then
                freezeConnection:Disconnect()
                freezeConnection = nil
            end
        end
    end,
})
-- ── Fly（カメラ基準・重力キャンセル） ────────────────
local flyEnabled = false
local flySpeed = 50
local flyMovement = { W=false, A=false, S=false, D=false, Space=false, LeftShift=false }
PlayerTab:CreateToggle({
    Name = "フライ (Fly)",
    CurrentValue = false,
    Callback = function(v) flyEnabled = v end,
})
PlayerTab:CreateSlider({
    Name = "Fly 速度",
    Range = {10, 2000},
    Increment = 5,
    CurrentValue = flySpeed,
    Callback = function(v) flySpeed = v end,
})
-- キー入力監視
UserInputService.InputBegan:Connect(function(input, gameProcessed)
    if gameProcessed then return end
    if input.UserInputType ~= Enum.UserInputType.Keyboard then return end
    local key = input.KeyCode.Name
    if flyMovement[key] ~= nil then
        flyMovement[key] = true
    end
end)
UserInputService.InputEnded:Connect(function(input, gameProcessed)
    if gameProcessed then return end
    if input.UserInputType ~= Enum.UserInputType.Keyboard then return end
    local key = input.KeyCode.Name
    if flyMovement[key] ~= nil then
        flyMovement[key] = false
    end
end)
-- Fly本体処理
RunService.RenderStepped:Connect(function(dt)
    if not flyEnabled then return end
    local _, hum, root = getCharacter()
    if not root or not hum then return end
    root.AssemblyLinearVelocity = Vector3.zero
    root.AssemblyAngularVelocity = Vector3.zero
    local cam = Workspace.CurrentCamera
    local direction = Vector3.zero
    if flyMovement.W then direction += cam.CFrame.LookVector end
    if flyMovement.S then direction -= cam.CFrame.LookVector end
    if flyMovement.A then direction -= cam.CFrame.RightVector end
    if flyMovement.D then direction += cam.CFrame.RightVector end
    if flyMovement.Space then direction += Vector3.new(0,1,0) end
    if flyMovement.LeftShift then direction -= Vector3.new(0,1,0) end
    if direction.Magnitude > 0 then
        root.CFrame += direction.Unit * flySpeed * dt
    end
end)
-- ── 位置保存・テレポート（簡易1スロット） ───────────
local savedPosition
PlayerTab:CreateButton({
    Name = "現在位置を保存",
    Callback = function()
        local _, _, hrp = getCharacter()
        if hrp then
            savedPosition = hrp.CFrame
            Rayfield:Notify({
                Title = "位置保存",
                Content = "現在の座標を記憶しました",
                Duration = 2.5
            })
        end
    end,
})
PlayerTab:CreateButton({
    Name = "保存位置へテレポート",
    Callback = function()
        if not savedPosition then
            Rayfield:Notify({Title="エラー", Content="まだ位置が保存されていません", Duration=3})
            return
        end
        local _, _, hrp = getCharacter()
        if hrp then
            hrp.CFrame = savedPosition
        end
    end,
})
-- ── 常時反映（WalkSpeedなど） ───────────────────────
RunService.RenderStepped:Connect(function()
    if speedEnabled then
        local _, hum = getCharacter()
        if hum then
            hum.WalkSpeed = speedValue
        end
    end
end)
--━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
-- Tab: ESP (Safe/Unsafe削除版: NameESPにLv表示のみ追加 + 全機能)
--━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
local ESPTab = Window:CreateTab("ESP", 4483362458)

-- ── Services ────────────────────────────────────────
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local Lighting = game:GetService("Lighting")
local Camera = workspace.CurrentCamera
local LocalPlayer = Players.LocalPlayer

-- ── Level 取得関数のみ残す ─────────────────────────
local function getLevel(player)
    local success, lvl = pcall(function()
        local leaderstats = player:FindFirstChild("leaderstats")
        if leaderstats and leaderstats:FindFirstChild("Level") then
            return leaderstats.Level.Value
        end
        local data = player:FindFirstChild("Data")
        if data and data:FindFirstChild("Level") then
            return data.Level.Value
        end
        return 0
    end)
    return success and lvl or 0
end

-- ── 状態フラグ ──────────────────────────────────────
local Settings = {
    AllyHighlight = false,
    EnemyHighlight = false,
    NameESP = false,
    LineESP = false,
    FullBright = false,
    WorldXRay = false,
    PlayerXRay = false,
    ItemHighlight = false,
    ChestHighlight = false,
    HitboxExtender = false,
  
    WorldXRayAlpha = 0.6,
    PlayerXRayAlpha = 0.6,
  
    ClockGUI = false,
}

-- ── 管理テーブル ────────────────────────────────────
local Highlights = {} -- char → Highlight
local NameDrawings = {} -- player → {name=Text, level=Text}
local LineDrawings = {} -- player → Drawing Line
local ChestHighlights = {} -- model → Highlight
local OriginalHRPSizes = {} -- player → Vector3 (他人のHRP)

-- ── ユーティリティ ──────────────────────────────────
local function IsEnemy(player)
    if not LocalPlayer.Team or not player.Team then
        return player ~= LocalPlayer
    end
    return player.Team ~= LocalPlayer.Team
end

local function CreateHighlight(target, color)
    if Highlights[target] then return end
  
    local hl = Instance.new("Highlight")
    hl.FillColor = color
    hl.OutlineColor = Color3.new(1,1,1)
    hl.FillTransparency = 0.5
    hl.Parent = target
    Highlights[target] = hl
end

local function RemoveHighlight(target)
    if Highlights[target] then
        Highlights[target]:Destroy()
        Highlights[target] = nil
    end
end

-- ── Name + Line + Lv ESP ───────────────────────────
RunService.RenderStepped:Connect(function()
    for _, player in Players:GetPlayers() do
        if player == LocalPlayer then continue end
      
        local char = player.Character
        if not char then continue end
      
        local hrp = char:FindFirstChild("HumanoidRootPart")
        local head = char:FindFirstChild("Head")
        if not hrp or not head then 
            if NameDrawings[player] then
                for _, d in pairs(NameDrawings[player]) do d.Visible = false end
            end
            if LineDrawings[player] then LineDrawings[player].Visible = false end
            continue 
        end
      
        local rootPos, onScreen = Camera:WorldToViewportPoint(hrp.Position)
        local headPos = Camera:WorldToViewportPoint(head.Position + Vector3.new(0, 0.5, 0))
      
        if not onScreen then
            if NameDrawings[player] then
                for _, d in pairs(NameDrawings[player]) do d.Visible = false end
            end
            if LineDrawings[player] then LineDrawings[player].Visible = false end
            continue
        end
      
        local distance = (Camera.CFrame.Position - hrp.Position).Magnitude
      
        -- Name + Lv ESP
        if Settings.NameESP then
            if not NameDrawings[player] then
                local nameTxt = Drawing.new("Text")
                nameTxt.Center = true
                nameTxt.Outline = true
                nameTxt.Size = 16
                nameTxt.Font = 2
                
                local levelTxt = Drawing.new("Text")
                levelTxt.Center = true
                levelTxt.Outline = true
                levelTxt.Size = 15
                levelTxt.Font = 2
                levelTxt.Color = Color3.new(0, 1, 1)  -- 青
                
                NameDrawings[player] = {name = nameTxt, level = levelTxt}
            end
          
            local drawings = NameDrawings[player]
            
            drawings.name.Visible = true
            drawings.name.Text = string.format("%s | %.0fm", player.Name, distance)
            drawings.name.Position = Vector2.new(rootPos.X, headPos.Y - 30)
            drawings.name.Color = IsEnemy(player) and Color3.new(1,0,0) or Color3.new(0,1,0)
            
            local lvl = getLevel(player)
            drawings.level.Text = "Lv." .. tostring(lvl)
            drawings.level.Position = Vector2.new(rootPos.X, headPos.Y - 50)
            drawings.level.Visible = true
        elseif NameDrawings[player] then
            for _, d in pairs(NameDrawings[player]) do d.Visible = false end
        end
      
        -- Line ESP
        if Settings.LineESP then
            if not LineDrawings[player] then
                local line = Drawing.new("Line")
                line.Thickness = 1.5
                LineDrawings[player] = line
            end
          
            local line = LineDrawings[player]
            line.Visible = true
            line.From = Vector2.new(Camera.ViewportSize.X / 2, Camera.ViewportSize.Y)
            line.To = Vector2.new(rootPos.X, rootPos.Y)
            line.Color = Color3.new(1, 0, 0)
        elseif LineDrawings[player] then
            LineDrawings[player].Visible = false
        end
    end
end)

-- プレイヤー退出時のクリーンアップ
Players.PlayerRemoving:Connect(function(player)
    if NameDrawings[player] then
        for _, drawing in pairs(NameDrawings[player]) do
            drawing:Remove()
        end
        NameDrawings[player] = nil
    end
    if LineDrawings[player] then
        LineDrawings[player]:Remove()
        LineDrawings[player] = nil
    end
    if player.Character then
        RemoveHighlight(player.Character)
    end
    OriginalHRPSizes[player] = nil
end)

-- ── Clock 表示 ──────────────────────────────────────
local ClockLabel = ESPTab:CreateLabel("Time: --:--")
local ClockGui = Instance.new("ScreenGui")
ClockGui.Name = "ClockGui"
ClockGui.ResetOnSpawn = false
ClockGui.Enabled = false
ClockGui.Parent = game:GetService("CoreGui")

do
    local frame = Instance.new("Frame")
    frame.Size = UDim2.new(0, 220, 0, 60)
    frame.Position = UDim2.new(0.5, -110, 0, 40)
    frame.BackgroundColor3 = Color3.fromRGB(15,15,15)
    frame.BackgroundTransparency = 0.15
    frame.Parent = ClockGui
  
    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 12)
    corner.Parent = frame
  
    local label = Instance.new("TextLabel")
    label.Size = UDim2.new(1, -10, 1, -10)
    label.Position = UDim2.new(0, 5, 0, 5)
    label.BackgroundTransparency = 1
    label.Font = Enum.Font.GothamBold
    label.TextScaled = true
    label.TextColor3 = Color3.fromRGB(255,255,255)
    label.Text = "--:--"
    label.Parent = frame
end

local function FormatTime(t)
    local h = math.floor(t)
    local m = math.floor((t - h) * 60)
    return string.format("%02d:%02d", h, m)
end

local function IsNight(t)
    return t >= 18 or t < 7
end

RunService.Heartbeat:Connect(function()
    local t = Lighting.ClockTime
    local timeStr = FormatTime(t)
    local state = IsNight(t) and "Night" or "Morning"
  
    ClockLabel:Set("Time: " .. timeStr .. " (" .. state .. ")")
  
    if Settings.ClockGUI and ClockGui:FindFirstChild("Frame", true) then
        ClockGui.Frame.TextLabel.Text = timeStr .. " (" .. state .. ")"
    end
end)

ESPTab:CreateToggle({
    Name = "Clock GUI 表示",
    CurrentValue = false,
    Callback = function(v)
        Settings.ClockGUI = v
        ClockGui.Enabled = v
    end,
})

-- ── 自分の現在位置表示 ──────────────────────────────
local PositionLabel = ESPTab:CreateLabel("Position: --, --, --")
RunService.RenderStepped:Connect(function()
    local char = LocalPlayer.Character
    local hrp = char and char:FindFirstChild("HumanoidRootPart")
    if hrp then
        local p = hrp.Position
        PositionLabel:Set(string.format("Position: X %.1f | Y %.1f | Z %.1f", p.X, p.Y, p.Z))
    else
        PositionLabel:Set("Position: Loading...")
    end
end)

-- ── FullBright ──────────────────────────────────────
local fullBrightConn
local OriginalLightingProps = {
    Brightness = Lighting.Brightness,
    ClockTime = Lighting.ClockTime,
    FogEnd = Lighting.FogEnd,
}

ESPTab:CreateToggle({
    Name = "FullBright",
    CurrentValue = false,
    Callback = function(enabled)
        Settings.FullBright = enabled
      
        if enabled then
            if fullBrightConn then fullBrightConn:Disconnect() end
            fullBrightConn = RunService.RenderStepped:Connect(function()
                Lighting.Brightness = 5
                Lighting.ClockTime = 12
                Lighting.FogEnd = 1e9
            end)
        else
            if fullBrightConn then
                fullBrightConn:Disconnect()
                fullBrightConn = nil
            end
            Lighting.Brightness = OriginalLightingProps.Brightness
            Lighting.ClockTime = OriginalLightingProps.ClockTime
            Lighting.FogEnd = OriginalLightingProps.FogEnd
        end
    end,
})

-- ── Fog / Atmosphere 無効化 ─────────────────────────
local fogEnabled = false
local fogConn
local OriginalFog = {
    Start = Lighting.FogStart,
    End = Lighting.FogEnd,
}
local SavedAtmospheres = {}
local SavedEffects = {}

do
    for _, child in Lighting:GetChildren() do
        if child:IsA("Atmosphere") then
            table.insert(SavedAtmospheres, child:Clone())
        elseif child:IsA("BloomEffect") or child:IsA("ColorCorrectionEffect") or
               child:IsA("DepthOfFieldEffect") or child:IsA("SunRaysEffect") then
            SavedEffects[child] = child.Enabled
        end
    end
end

local function DisableFogAndEffects()
    Lighting.FogStart = 1e9
    Lighting.FogEnd = 1e9
  
    for _, child in Lighting:GetChildren() do
        if child:IsA("Atmosphere") then
            child:Destroy()
        elseif SavedEffects[child] ~= nil then
            child.Enabled = false
        end
    end
end

ESPTab:CreateToggle({
    Name = "Fog / Atmosphere 無効化",
    CurrentValue = false,
    Callback = function(v)
        fogEnabled = v
        if v then
            DisableFogAndEffects()
            if fogConn then fogConn:Disconnect() end
            fogConn = RunService.Heartbeat:Connect(DisableFogAndEffects)
        else
            if fogConn then
                fogConn:Disconnect()
                fogConn = nil
            end
            Lighting.FogStart = OriginalFog.Start
            Lighting.FogEnd = OriginalFog.End
          
            for _, child in Lighting:GetChildren() do
                if child:IsA("Atmosphere") then child:Destroy() end
            end
            for _, clone in SavedAtmospheres do
                clone:Clone().Parent = Lighting
            end
            for effect, wasEnabled in SavedEffects do
                if effect and effect.Parent then
                    effect.Enabled = wasEnabled
                end
            end
        end
    end,
})

-- ── ハイライト ──────────────────────────────────────
ESPTab:CreateToggle({
    Name = "味方ハイライト",
    CurrentValue = false,
    Callback = function(v)
        Settings.AllyHighlight = v
    end,
})

ESPTab:CreateToggle({
    Name = "敵ハイライト",
    CurrentValue = false,
    Callback = function(v)
        Settings.EnemyHighlight = v
    end,
})

RunService.Stepped:Connect(function()
    for _, player in Players:GetPlayers() do
        if player == LocalPlayer or not player.Character then continue end
      
        if IsEnemy(player) and Settings.EnemyHighlight then
            CreateHighlight(player.Character, Color3.new(1,0,0))
        elseif not IsEnemy(player) and Settings.AllyHighlight then
            CreateHighlight(player.Character, Color3.new(0,1,0))
        else
            RemoveHighlight(player.Character)
        end
    end
end)

-- ── Name / Line ESP トグル ──────────────────────────
ESPTab:CreateToggle({
    Name = "名前ESP (Lv表示付き)",
    CurrentValue = false,
    Callback = function(v) 
        Settings.NameESP = v 
    end,
})

ESPTab:CreateToggle({
    Name = "線ESP",
    CurrentValue = false,
    Callback = function(v) Settings.LineESP = v end,
})

-- ── X-Ray ───────────────────────────────────────────
ESPTab:CreateToggle({
    Name = "ワールド X-Ray",
    CurrentValue = false,
    Callback = function(v)
        Settings.WorldXRay = v
        for _, part in Workspace:GetDescendants() do
            if part:IsA("BasePart") then
                part.LocalTransparencyModifier = v and Settings.WorldXRayAlpha or 0
            end
        end
    end,
})

ESPTab:CreateSlider({
    Name = "ワールド X-Ray 透明度",
    Range = {0, 0.95},
    Increment = 0.05,
    Suffix = "Alpha",
    CurrentValue = Settings.WorldXRayAlpha,
    Callback = function(v)
        Settings.WorldXRayAlpha = v
        if not Settings.WorldXRay then return end
        for _, part in Workspace:GetDescendants() do
            if part:IsA("BasePart") then
                part.LocalTransparencyModifier = v
            end
        end
    end,
})

ESPTab:CreateToggle({
    Name = "プレイヤー X-Ray",
    CurrentValue = false,
    Callback = function(v)
        Settings.PlayerXRay = v
        for _, player in Players:GetPlayers() do
            if player == LocalPlayer or not player.Character then continue end
            for _, part in player.Character:GetDescendants() do
                if part:IsA("BasePart") then
                    part.LocalTransparencyModifier = v and Settings.PlayerXRayAlpha or 0
                end
            end
        end
    end,
})

ESPTab:CreateSlider({
    Name = "プレイヤー X-Ray 透明度",
    Range = {0, 0.95},
    Increment = 0.05,
    Suffix = "Alpha",
    CurrentValue = Settings.PlayerXRayAlpha,
    Callback = function(v)
        Settings.PlayerXRayAlpha = v
        if not Settings.PlayerXRay then return end
        for _, player in Players:GetPlayers() do
            if player == LocalPlayer or not player.Character then continue end
            for _, part in player.Character:GetDescendants() do
                if part:IsA("BasePart") then
                    part.LocalTransparencyModifier = v
                end
            end
        end
    end,
})

-- ── アイテム / チェスト ハイライト ──────────────────
local function HighlightModelsByKeyword(keyword, color)
    for _, obj in Workspace:GetDescendants() do
        if obj:IsA("Model") and string.find(obj.Name:lower(), keyword) then
            local primary = obj.PrimaryPart or obj:FindFirstChildWhichIsA("BasePart")
            if primary then
                CreateHighlight(primary, color)
            end
        end
    end
end

ESPTab:CreateToggle({
    Name = "アイテムハイライト",
    CurrentValue = false,
    Callback = function(v)
        Settings.ItemHighlight = v
        if v then
            HighlightModelsByKeyword("item", Color3.fromRGB(0, 255, 255))
        end
    end,
})

ESPTab:CreateToggle({
    Name = "チェストハイライト",
    CurrentValue = false,
    Callback = function(v)
        Settings.ChestHighlight = v
        if v then
            for _, obj in Workspace:GetDescendants() do
                if obj:IsA("Model") and string.find(obj.Name:lower(), "chest") then
                    if not ChestHighlights[obj] then
                        local hl = Instance.new("Highlight")
                        hl.FillColor = Color3.fromRGB(255, 215, 0)
                        hl.FillTransparency = 0.4
                        hl.Parent = obj
                        ChestHighlights[obj] = hl
                    end
                end
            end
        else
            for _, hl in pairs(ChestHighlights) do
                if hl then hl:Destroy() end
            end
            table.clear(ChestHighlights)
        end
    end,
})

-- ── Hitbox Extender ─────────────────────────────────
ESPTab:CreateToggle({
    Name = "HitBox 表示（拡張）",
    CurrentValue = false,
    Callback = function(v)
        Settings.HitboxExtender = v
        for _, player in Players:GetPlayers() do
            if player == LocalPlayer or not player.Character then continue end
          
            local hrp = player.Character:FindFirstChild("HumanoidRootPart")
            if not hrp then continue end
          
            if v then
                if not OriginalHRPSizes[player] then
                    OriginalHRPSizes[player] = hrp.Size
                end
              
                local base = math.max(hrp.Size.X, hrp.Size.Y, hrp.Size.Z) / 2
                local newSize = Vector3.new(base*2, base*2, base*2) * HitboxScale
              
                hrp.Size = newSize
                hrp.Transparency = 0.5
                hrp.CanCollide = false
                hrp.Color = IsEnemy(player) and Color3.new(1,0,0) or Color3.new(1,1,1)
            else
                if OriginalHRPSizes[player] then
                    hrp.Size = OriginalHRPSizes[player]
                end
                hrp.Transparency = 1
                hrp.CanCollide = true
            end
        end
    end,
})

local HitboxScale = 1
ESPTab:CreateSlider({
    Name = "HitBox 倍率",
    Range = {1, 20},
    Increment = 0.1,
    Suffix = "倍",
    CurrentValue = HitboxScale,
    Callback = function(v)
        HitboxScale = v
        if not Settings.HitboxExtender then return end
      
        for _, player in Players:GetPlayers() do
            if player == LocalPlayer or not player.Character then continue end
            local hrp = player.Character:FindFirstChild("HumanoidRootPart")
            if hrp and OriginalHRPSizes[player] then
                local base = math.max(OriginalHRPSizes[player].X, OriginalHRPSizes[player].Y, OriginalHRPSizes[player].Z) / 2
                hrp.Size = Vector3.new(base*2, base*2, base*2) * v
                hrp.Transparency = 0.5
            end
        end
    end,
})

-- ── 自分の攻撃範囲拡大 ──────────────────────────────
local SelfHitboxOriginalSize
local SelfHitboxScale = 1
ESPTab:CreateSlider({
    Name = "自分の攻撃範囲倍率",
    Range = {1, 10},
    Increment = 0.1,
    Suffix = "倍",
    CurrentValue = SelfHitboxScale,
    Callback = function(v)
        SelfHitboxScale = v
      
        local char = LocalPlayer.Character
        local hrp = char and char:FindFirstChild("HumanoidRootPart")
        if not hrp then return end
      
        if not SelfHitboxOriginalSize then
            SelfHitboxOriginalSize = hrp.Size
        end
      
        hrp.Size = SelfHitboxOriginalSize * v
    end,
})

-- ── Fruit VFX Color ─────────────────────────────────
local VFXEnabled = false
local RainbowEnabled = false
local SelectedVFXColor = Color3.fromRGB(255, 0, 0)
local hue = 0
local const RAINBOW_SPEED = 0.12
local const HUE_OFFSET = 0.08

local function ScanFruitVFX()
    local results = {}
    for _, child in LocalPlayer:GetChildren() do
        if child:IsA("Folder") and child.Name:find("FruitVFXColor") then
            local container = child:FindFirstChild("Shifted") or child:FindFirstChild("Default")
            if not container then continue end
          
            local attrs = {}
            for name, value in container:GetAttributes() do
                if typeof(value) == "Color3" then
                    table.insert(attrs, name)
                end
            end
          
            if #attrs > 0 then
                table.insert(results, { folder = container, attrs = attrs })
            end
        end
    end
    return results
end

local function ApplyVFXColor()
    if not VFXEnabled and not RainbowEnabled then return end
  
    local targets = ScanFruitVFX()
    if #targets == 0 then return end
  
    if RainbowEnabled then
        hue = (hue + RAINBOW_SPEED) % 1
        for _, data in targets do
            for i, attr in data.attrs do
                local h = (hue + (i-1) * HUE_OFFSET) % 1
                data.folder:SetAttribute(attr, Color3.fromHSV(h, 1, 1))
            end
        end
    elseif VFXEnabled then
        for _, data in targets do
            for _, attr in data.attrs do
                data.folder:SetAttribute(attr, SelectedVFXColor)
            end
        end
    end
end

task.spawn(function()
    while true do
        task.wait(0.6)
        ApplyVFXColor()
    end
end)

ESPTab:CreateToggle({
    Name = "Fruit VFX Color",
    CurrentValue = false,
    Callback = function(v)
        VFXEnabled = v
        ApplyVFXColor()
    end,
})

ESPTab:CreateToggle({
    Name = "Rainbow VFX",
    CurrentValue = false,
    Callback = function(v)
        RainbowEnabled = v
        ApplyVFXColor()
    end,
})

ESPTab:CreateColorPicker({
    Name = "VFX Color",
    Color = SelectedVFXColor,
    Callback = function(color)
        SelectedVFXColor = color
        if VFXEnabled and not RainbowEnabled then
            ApplyVFXColor()
        end
    end,
})



--━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
-- Tab: 戦闘 (Combat) - 完全版（エラー対策強化）
--━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
local CombatTab = Window:CreateTab("戦闘", 4483362458)
-- ── 状態管理 ────────────────────────────────────────
local CombatSettings = {
    Invisible = false,
    KeybindEnabled = false,
    InvisibleKey = Enum.KeyCode.G,
    TracerEnabled = false,
    FollowActive = false,
    FollowMode = nil, -- "normal", "v2", "under"
}
-- ── 共有変数 ────────────────────────────────────────
local SelectedTarget = nil
local OriginalFollowPos = nil
local InvisibleParts = {}
local CharacterRef = nil
local HumanoidRef = nil
local RootPartRef = nil
local LastKnownCharacter = nil -- 変更検知用
-- ── キャラクター参照更新（Heartbeatで常時監視） ─────
local function TryUpdateCharacterRefs()
    local currentChar = LocalPlayer.Character
   
    -- 変化なし → 何もしない（パフォーマンス重視）
    if currentChar == LastKnownCharacter then return end
   
    -- Characterが消えた → クリア
    if not currentChar or not currentChar.Parent then
        CharacterRef = nil
        HumanoidRef = nil
        RootPartRef = nil
        InvisibleParts = {}
        LastKnownCharacter = nil
        return
    end
   
    -- 新しいCharacter → 少し待ってから取得
    task.wait(0.1) -- リスポーン直後の不安定さを回避
   
    if not currentChar.Parent then return end
   
    CharacterRef = currentChar
    HumanoidRef = currentChar:FindFirstChild("Humanoid")
    RootPartRef = currentChar:FindFirstChild("HumanoidRootPart")
   
    -- まだ揃っていない場合 → 次フレームに持ち越し
    if not HumanoidRef or not RootPartRef then
        CharacterRef = nil
        return
    end
   
    -- パーツ収集
    InvisibleParts = {}
    for _, obj in ipairs(currentChar:GetDescendants()) do
        if obj:IsA("BasePart") and obj.Transparency == 0 then
            table.insert(InvisibleParts, obj)
        end
    end
   
    -- InvisibleがONなら即適用
    if CombatSettings.Invisible then
        for _, part in ipairs(InvisibleParts) do
            pcall(function() part.Transparency = 0.5 end)
        end
    end
   
    LastKnownCharacter = currentChar
    -- print("[Invisible] 参照更新成功") -- デバッグ用（必要なら有効化）
end
-- 初回実行
TryUpdateCharacterRefs()
-- 毎フレームでキャラクター状態をチェック
RunService.Heartbeat:Connect(TryUpdateCharacterRefs)
-- ── Invisible 切り替え ──────────────────────────────
local function ToggleInvisible(enabled)
    CombatSettings.Invisible = enabled
   
    -- パーツがまだない場合でも、Heartbeatがすぐに更新してくれるので待たない
    for _, part in ipairs(InvisibleParts) do
        pcall(function()
            part.Transparency = enabled and 0.5 or 0
        end)
    end
   
    Rayfield:Notify({
        Title = "Invisible",
        Content = enabled and "透明化 ON" or "透明化 OFF",
        Duration = 3
    })
end
-- ── Noclip ──────────────────────────────────────────
local NoclipConn
local OriginalCanCollideCache = {}
local function EnableNoclip()
    if NoclipConn then return end
    if not CharacterRef then return end
   
    OriginalCanCollideCache = {}
    for _, part in ipairs(CharacterRef:GetDescendants()) do
        if part:IsA("BasePart") then
            OriginalCanCollideCache[part] = part.CanCollide
        end
    end
   
    NoclipConn = RunService.Stepped:Connect(function()
        if not CharacterRef then return end
        for _, part in ipairs(CharacterRef:GetDescendants()) do
            if part:IsA("BasePart") then
                part.CanCollide = false
            end
        end
    end)
end
local function DisableNoclip()
    if NoclipConn then
        NoclipConn:Disconnect()
        NoclipConn = nil
    end
   
    if not CharacterRef then return end
   
    for part, can in pairs(OriginalCanCollideCache) do
        if part and part.Parent then
            pcall(function() part.CanCollide = can end)
        end
    end
    table.clear(OriginalCanCollideCache)
end
-- ── Follow ユーティリティ ───────────────────────────
local function GetHRP(char) return char and char:FindFirstChild("HumanoidRootPart") end
local function GetHum(char) return char and char:FindFirstChildOfClass("Humanoid") end
local function StartFollow(mode)
    if not SelectedTarget or not SelectedTarget.Character then return end
   
    CombatSettings.FollowActive = true
    CombatSettings.FollowMode = mode
   
    local myHRP = GetHRP(LocalPlayer.Character)
    if myHRP and not OriginalFollowPos then
        OriginalFollowPos = myHRP.CFrame
    end
end
local function StopFollow()
    CombatSettings.FollowActive = false
    CombatSettings.FollowMode = nil
   
    local myHRP = GetHRP(LocalPlayer.Character)
    local myHum = GetHum(LocalPlayer.Character)
    if myHRP and OriginalFollowPos then
        myHRP.CFrame = OriginalFollowPos
        if myHum then myHum.PlatformStand = false end
        DisableNoclip()
        OriginalFollowPos = nil
    end
end
-- ── Tracer ──────────────────────────────────────────
local TracerLine = Drawing.new("Line")
TracerLine.Visible = false
TracerLine.Thickness = 2
TracerLine.Transparency = 1
TracerLine.Color = Color3.fromRGB(0, 255, 255)
-- ── GUI コントロール ────────────────────────────────
CombatTab:CreateButton({
    Name = "選択中のプレイヤーへ TP",
    Callback = function()
        if not SelectedTarget or not SelectedTarget.Character then
            Rayfield:Notify({Title="エラー", Content="ターゲットが無効です", Duration=3})
            return
        end
        local targetHRP = GetHRP(SelectedTarget.Character)
        if targetHRP then
            LocalPlayer.Character:PivotTo(targetHRP.CFrame * CFrame.new(0, 0, 3))
        end
    end,
})
CombatTab:CreateToggle({
    Name = "普通の張り付き",
    Callback = function(v) if v then StartFollow("normal") else StopFollow() end end,
})
CombatTab:CreateToggle({
    Name = "張り付き v2 (BloxFruit対応)",
    Callback = function(v) if v then StartFollow("v2") else StopFollow() end end,
})
CombatTab:CreateToggle({
    Name = "下向き張り付き",
    Callback = function(v) if v then StartFollow("under") else StopFollow() end end,
})
CombatTab:CreateToggle({
    Name = "ターゲット線 (Tracer)",
    Callback = function(v)
        CombatSettings.TracerEnabled = v
        if not v then TracerLine.Visible = false end
    end,
})
CombatTab:CreateToggle({
    Name = "Invisible (ゲームによっては無効)",
    CurrentValue = false,
    Callback = function(v) ToggleInvisible(v) end,
})
CombatTab:CreateToggle({
    Name = "Invisible キー切替を有効化",
    CurrentValue = false,
    Callback = function(v) CombatSettings.KeybindEnabled = v end,
})
CombatTab:CreateInput({
    Name = "Invisible キー (例: G)",
    PlaceholderText = "例: G",
    RemoveTextAfterFocusLost = true,
    Callback = function(text)
        local upper = text:upper()
        local key = Enum.KeyCode[upper]
        if key then
            CombatSettings.InvisibleKey = key
            Rayfield:Notify({
                Title = "設定完了",
                Content = "Invisibleキーを " .. upper .. " に変更しました",
                Duration = 3
            })
        else
            Rayfield:Notify({Title="エラー", Content="無効なキー名です", Duration=3})
        end
    end,
})
-- ── プレイヤー一覧 ──────────────────────────────────
CombatTab:CreateSection("プレイヤー一覧")
local PlayerButtons = {}
local function GetHP(plr)
    local hum = plr.Character and plr.Character:FindFirstChildOfClass("Humanoid")
    return hum and math.floor(hum.Health) or 0, hum and math.floor(hum.MaxHealth) or 0
end
local function CreateTargetButton(plr)
    local hp, maxhp = GetHP(plr)
    local btn = CombatTab:CreateButton({
        Name = plr.Name .. " [" .. hp .. "/" .. maxhp .. "]",
        Callback = function()
            SelectedTarget = plr
            Rayfield:Notify({
                Title = "ターゲット選択",
                Content = plr.Name .. " を選択しました",
                Duration = 3
            })
        end,
    })
    PlayerButtons[plr] = btn
end
local function RefreshPlayerList()
    local active = {}
    for _, p in ipairs(Players:GetPlayers()) do
        if p ~= LocalPlayer then
            active[p] = true
            if not PlayerButtons[p] then
                CreateTargetButton(p)
            end
        end
    end
   
    for plr, btn in pairs(PlayerButtons) do
        if not active[plr] then
            pcall(function() btn:Remove() end)
            PlayerButtons[plr] = nil
        end
    end
end
RefreshPlayerList()
Players.PlayerAdded:Connect(RefreshPlayerList)
Players.PlayerRemoving:Connect(RefreshPlayerList)
-- ── Invisibleキー入力 ───────────────────────────────
UserInputService.InputBegan:Connect(function(input, gpe)
    if gpe or not CombatSettings.KeybindEnabled then return end
    if input.UserInputType == Enum.UserInputType.Keyboard and input.KeyCode == CombatSettings.InvisibleKey then
        ToggleInvisible(not CombatSettings.Invisible)
    end
end)
-- ── Invisible 位置ハック ─────────────────────────────
local SAFE_DROP_Y = -200000
RunService.Heartbeat:Connect(function()
    if not CombatSettings.Invisible then return end
    if not RootPartRef or not RootPartRef.Parent or not HumanoidRef then return end
   
    local originalCF = RootPartRef.CFrame
    local camOffset = HumanoidRef.CameraOffset
   
    local hiddenCF = originalCF * CFrame.new(0, SAFE_DROP_Y, 0)
    RootPartRef.CFrame = hiddenCF
    HumanoidRef.CameraOffset = hiddenCF:ToObjectSpace(CFrame.new(originalCF.Position)).Position
   
    RunService.RenderStepped:Wait()
   
    if RootPartRef and RootPartRef.Parent then
        RootPartRef.CFrame = originalCF
        HumanoidRef.CameraOffset = camOffset
    end
end)
-- ── Follow + Tracer ─────────────────────────────────
RunService.RenderStepped:Connect(function(dt)
    if not CharacterRef or not HumanoidRef or not RootPartRef then return end
   
    if CombatSettings.FollowActive then
        RootPartRef.AssemblyLinearVelocity = Vector3.zero
    end
   
    if CombatSettings.FollowActive and SelectedTarget and SelectedTarget.Character then
        local targetHRP = GetHRP(SelectedTarget.Character)
        if targetHRP then
            if CombatSettings.FollowMode == "normal" then
                RootPartRef.CFrame = targetHRP.CFrame * CFrame.new(0, 0, 7)
               
            elseif CombatSettings.FollowMode == "v2" then
                local delta = targetHRP.Position - RootPartRef.Position
                local dist = delta.Magnitude
                local speed = 300
               
                if dist > 200 then
                    RootPartRef.CFrame = RootPartRef.CFrame:Lerp(
                        CFrame.new(RootPartRef.Position + delta.Unit * speed * dt), 1
                    )
                else
                    RootPartRef.CFrame = RootPartRef.CFrame:Lerp(
                        targetHRP.CFrame * CFrame.new(0, 0, 7), 0.2
                    )
                end
               
            elseif CombatSettings.FollowMode == "under" then
                if not NoclipConn then EnableNoclip() end
                HumanoidRef.PlatformStand = true
               
                local goal = targetHRP.CFrame * CFrame.new(0, -12, 0) * CFrame.Angles(math.rad(90), 0, 0)
                RootPartRef.CFrame = RootPartRef.CFrame:Lerp(goal, 0.3)
            end
        end
    else
        if HumanoidRef then HumanoidRef.PlatformStand = false end
        if NoclipConn then DisableNoclip() end
    end
   
    -- Tracer
    if CombatSettings.TracerEnabled and SelectedTarget and SelectedTarget.Character then
        local targetHRP = GetHRP(SelectedTarget.Character)
        if targetHRP then
            local p1, on1 = Camera:WorldToViewportPoint(RootPartRef.Position)
            local p2, on2 = Camera:WorldToViewportPoint(targetHRP.Position)
           
            TracerLine.Visible = on1 and on2
            if on1 and on2 then
                TracerLine.From = Vector2.new(p1.X, p1.Y)
                TracerLine.To = Vector2.new(p2.X, p2.Y)
            end
        else
            TracerLine.Visible = false
        end
    else
        TracerLine.Visible = false
    end
end)
-- ── HP更新 ──────────────────────────────────────────
RunService.Heartbeat:Connect(function()
    for plr, btn in pairs(PlayerButtons) do
        if not btn then continue end
       
        local hp, maxhp = GetHP(plr)
        pcall(function()
            btn:Set(plr.Name .. " [" .. hp .. "/" .. maxhp .. "]")
        end)
    end
end)
-- ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
-- Blox Fruits Tab (FastAttack + 自動銃射撃連携 / UIなし)
-- FastAttack ON → M1 + ShootGunEvent自動連射（範囲自動連動）
-- 銃連射間隔: 0.2秒固定 / 範囲: 通常 or 大仏状態に同期
-- ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

local BloxfruitTab = Window:CreateTab("Blox Fruits", 4483362458)
-- 設定変数（既存）
getgenv().FastM1V3 = false
getgenv().TargetMode = "敵Bot"
getgenv().RangeNormal = 80
getgenv().RangeBuddha = 500
getgenv().AttackInterval = 0.1
getgenv().MaxTargets = 40
getgenv().M1SpamEnabled = false

local RS = game:GetService("ReplicatedStorage")
local Players = game:GetService("Players")
local player = Players.LocalPlayer
local RunService = game:GetService("RunService")
local RegisterAttack, RegisterHit = nil, nil
local FastM1Thread = nil

-- Remote検索（変更なし）
local function findRemotes()
    pcall(function()
        local Modules = RS:FindFirstChild("Modules")
        if Modules then
            local Net = Modules:FindFirstChild("Net")
            if Net then
                RegisterAttack = Net:FindFirstChild("RE/RegisterAttack") or Net["RE/RegisterAttack"]
                RegisterHit = Net:FindFirstChild("RE/RegisterHit") or Net["RE/RegisterHit"]
                if RegisterHit and RegisterAttack then return true end
               
                local RE = Net:FindFirstChild("RE")
                if RE then
                    RegisterAttack = RE:FindFirstChild("RegisterAttack")
                    RegisterHit = RE:FindFirstChild("RegisterHit")
                    if RegisterHit and RegisterAttack then return true end
                end
            end
        end
       
        for _, obj in pairs(RS:GetDescendants()) do
            if obj:IsA("RemoteEvent") then
                if obj.Name:find("RegisterAttack") then RegisterAttack = obj end
                if obj.Name:find("RegisterHit") then RegisterHit = obj end
            end
        end
       
        if RegisterHit and RegisterAttack then return true end
    end)
    return false
end
findRemotes()
spawn(function()
    local attempts = 0
    while attempts < 30 do
        if findRemotes() then break end
        attempts = attempts + 1
        task.wait(1)
    end
    if not (RegisterHit and RegisterAttack) then
        Rayfield:Notify({Title = "エラー", Content = "Register Remote未発見。リロードを。", Duration = 6})
    end
end)

-- Statusラベル（M1用のみ残す）
local StatusLabel = BloxfruitTab:CreateLabel("Status: Ready | Targets: 0 | Range: N/A")

-- FastAttackトグル（銃部分を削除）
BloxfruitTab:CreateToggle({
    Name = "FastAttack (範囲連動)",
    CurrentValue = false,
    Callback = function(Value)
        getgenv().FastM1V3 = Value
        getgenv().M1SpamEnabled = Value
       
        if Value then
            if not (RegisterHit and RegisterAttack) then
                Rayfield:Notify({Title = "エラー", Content = "Remote未発見！待機後再トグル", Duration = 5})
                getgenv().FastM1V3 = false
                getgenv().M1SpamEnabled = false
                return
            end
           
            -- FastAttackスレッド（元のまま）
            FastM1Thread = task.spawn(function()
                local lastAttackTime = 0
                while getgenv().FastM1V3 do
                    pcall(function()
                        local char = player.Character
                        if not char or not char:FindFirstChild("HumanoidRootPart") or char.Humanoid.Health <= 0 then
                            task.wait(0.1)
                            return
                        end
                       
                        local HRP = char.HumanoidRootPart
                        local isBuddha = HRP.Size.Y > 12
                        local bodyEffects = char:FindFirstChild("BodyEffects")
                        if bodyEffects then
                            for _, v in pairs(bodyEffects:GetChildren()) do
                                if v.Name:lower():find("buddha") or v.Name:lower():find("transform") then
                                    isBuddha = true
                                    break
                                end
                            end
                        end
                       
                        local range = isBuddha and getgenv().RangeBuddha or getgenv().RangeNormal
                        local targets = {}
                       
                        local enemiesFolder = workspace:FindFirstChild("Enemies")
                        local mode = getgenv().TargetMode
                        local doEnemies = (mode == "敵Bot" or mode == "両方")
                        local doPlayers = (mode == "プレイヤー" or mode == "両方")
                       
                        if doEnemies and enemiesFolder then
                            for _, enemy in pairs(enemiesFolder:GetChildren()) do
                                local eHead = enemy:FindFirstChild("Head") or enemy:FindFirstChild("UpperTorso") or enemy:FindFirstChild("Torso")
                                local eHRP = enemy:FindFirstChild("HumanoidRootPart") or enemy:FindFirstChild("Torso") or enemy:FindFirstChild("UpperTorso")
                                local eHum = enemy:FindFirstChildOfClass("Humanoid")
                               
                                if eHRP and eHead and eHum and eHum.Health > 0 then
                                    local dist = (eHRP.Position - HRP.Position).Magnitude
                                    if dist <= range then
                                        table.insert(targets, {model = enemy, head = eHead, dist = dist})
                                    end
                                end
                            end
                        end
                       
                        if doPlayers then
                            for _, plr in pairs(Players:GetPlayers()) do
                                if plr ~= player and plr.Character then
                                    local pChar = plr.Character
                                    local pHRP = pChar:FindFirstChild("HumanoidRootPart")
                                    local pHead = pChar:FindFirstChild("Head")
                                    local pHum = pChar:FindFirstChildOfClass("Humanoid")
                                    if pHRP and pHead and pHum and pHum.Health > 0 then
                                        local dist = (pHRP.Position - HRP.Position).Magnitude
                                        if dist <= range then
                                            table.insert(targets, {model = pChar, head = pHead, dist = dist})
                                        end
                                    end
                                end
                            end
                        end
                       
                        table.sort(targets, function(a,b) return a.dist < b.dist end)
                       
                        local limitedTargets = {}
                        for i = 1, math.min(getgenv().MaxTargets, #targets) do
                            table.insert(limitedTargets, targets[i])
                        end
                       
                        if #limitedTargets > 0 then
                            local hitList = {}
                            for _, t in ipairs(limitedTargets) do
                                table.insert(hitList, {t.model, t.head})
                            end
                           
                            if tick() - lastAttackTime > 0.05 then
                                RegisterAttack:FireServer(0)
                                lastAttackTime = tick()
                            end
                           
                            RegisterHit:FireServer(limitedTargets[1].head, hitList)
                        end
                       
                        StatusLabel:Set("Status: ON | Targets: " .. #limitedTargets .. "/" .. getgenv().MaxTargets
                            .. " | Range: " .. range .. " | Mode: " .. mode)
                    end)
                   
                    task.wait(getgenv().AttackInterval + math.random(5,15)/100)
                end
            end)
           
            Rayfield:Notify({Title = "ON", Content = "FastAttack開始（範囲自動連動）", Duration = 4})
        else
            if FastM1Thread then task.cancel(FastM1Thread) FastM1Thread = nil end
           
            StatusLabel:Set("Status: OFF")
            Rayfield:Notify({Title = "OFF", Content = "FastAttack停止", Duration = 3})
        end
    end,
})

-- M1連打部分（変更なし）
spawn(function()
    while wait(0.1) do
        if getgenv().M1SpamEnabled then
            pcall(function()
                local char = player.Character
                if not char then return end
               
                local HRP = char:FindFirstChild("HumanoidRootPart")
                if not HRP then return end
               
                local range = getgenv().RangeNormal + 100
               
                local hasEnemyInRange = false
                local mode = getgenv().TargetMode
               
                if mode == "敵Bot" or mode == "両方" then
                    local enemies = workspace:FindFirstChild("Enemies")
                    if enemies then
                        for _, enemy in pairs(enemies:GetChildren()) do
                            local eHRP = enemy:FindFirstChild("HumanoidRootPart")
                            local eHum = enemy:FindFirstChild("Humanoid")
                            if eHRP and eHum and eHum.Health > 0 then
                                if (eHRP.Position - HRP.Position).Magnitude <= range then
                                    hasEnemyInRange = true
                                    break
                                end
                            end
                        end
                    end
                end
               
                if not hasEnemyInRange and (mode == "プレイヤー" or mode == "両方") then
                    for _, plr in pairs(Players:GetPlayers()) do
                        if plr ~= player and plr.Character then
                            local pChar = plr.Character
                            local pHRP = pChar:FindFirstChild("HumanoidRootPart")
                            local pHum = pChar:FindFirstChild("Humanoid")
                            if pHRP and pHum and pHum.Health > 0 then
                                if (pHRP.Position - HRP.Position).Magnitude <= range then
                                    hasEnemyInRange = true
                                    break
                                end
                            end
                        end
                    end
                end
               
                if hasEnemyInRange then
                    local tool = nil
                    for _, t in pairs(char:GetChildren()) do
                        if t:IsA("Tool") and t:FindFirstChild("LeftClickRemote") then
                            tool = t
                            break
                        end
                    end
                   
                    if tool and tool:FindFirstChild("LeftClickRemote") then
                        local remote = tool.LeftClickRemote
                        remote:FireServer(table.unpack({
                            [1] = Vector3.new(0.84, -0.035, -0.54),
                            [2] = 1,
                        }))
                    end
                end
            end)
            task.wait(math.random(2,5)/100)
        end
    end
end)

-- スライダー類（銃関連のものは削除済み）
BloxfruitTab:CreateSlider({
    Name = "通常状態の攻撃範囲",
    Range = {10, 80},
    Increment = 10,
    Suffix = " studs",
    CurrentValue = getgenv().RangeNormal,
    Callback = function(v) getgenv().RangeNormal = v end,
})

BloxfruitTab:CreateSlider({
    Name = "大仏状態の攻撃範囲",
    Range = {50, 500},
    Increment = 50,
    Suffix = " studs",
    CurrentValue = getgenv().RangeBuddha,
    Callback = function(v) getgenv().RangeBuddha = v end,
})

BloxfruitTab:CreateSlider({
    Name = "最大同時ターゲット数",
    Range = {1, 50},
    Increment = 5,
    Suffix = "体",
    CurrentValue = getgenv().MaxTargets,
    Callback = function(v) getgenv().MaxTargets = v end,
})

BloxfruitTab:CreateSlider({
    Name = "攻撃間隔",
    Range = {0.1, 0.9},
    Increment = 0.1,
    Suffix = "秒",
    CurrentValue = getgenv().AttackInterval,
    Callback = function(v) getgenv().AttackInterval = v end,
})

BloxfruitTab:CreateDropdown({
    Name = "ターゲットモード",
    Options = {"敵Bot", "プレイヤー", "両方"},
    CurrentOption = {"敵Bot"},
    Callback = function(option)
        getgenv().TargetMode = option[1]
    end,
})

-- No Lava Damage (そのまま)
local NoLavaEnabled = false
local function DisableLavaDamage()
    pcall(function()
        for _, folderName in pairs({"CircleIsland", "GhostShipInterior", "HydraIsland"}) do
            local island = workspace.Map:FindFirstChild(folderName)
            if island then
                local lava = island:FindFirstChild("LavaParts")
                if lava then
                    for _, obj in pairs(lava:GetDescendants()) do
                        pcall(function()
                            if obj:IsA("Script") or obj:IsA("LocalScript") then
                                if obj.Name:lower():find("lava") or obj.Name:lower():find("damage") then
                                    obj.Disabled = true
                                end
                            end
                            if obj:IsA("TouchTransmitter") or obj.Name == "TouchInterest" then obj:Destroy() end
                            if obj:IsA("BasePart") and (obj.Name:lower():find("lava") or obj.Material == Enum.Material.Lava) then
                                obj.CanTouch = false
                                obj.CanCollide = false
                            end
                        end)
                    end
                end
            end
        end
    end)
end

-- バリア設定
local BARRIER_THICKNESS = 30          -- 厚さ30 studs
local BARRIER_TOP_Y     = -4         -- バリアの上面Y座標（ここより下には絶対落ちない）
local BARRIER_SIZE_XZ   = 10000       -- X,Z方向の広さ（十分大きく）

local barrier = nil
local heartbeatConn = nil
local enabled = false

-- バリア作成/更新関数
local function updateBarrier(character)
    -- すでに存在したら一旦削除
    if barrier then
        barrier:Destroy()
        barrier = nil
    end

    if not enabled then return end
    if not character or not character:FindFirstChild("HumanoidRootPart") then return end

    barrier = Instance.new("Part")
    barrier.Name = "AntiFallBarrier"
    barrier.Size = Vector3.new(BARRIER_SIZE_XZ, BARRIER_THICKNESS, BARRIER_SIZE_XZ)
    
    -- 中心Y座標 = 上面Y - 厚さ/2
    local centerY = BARRIER_TOP_Y - (BARRIER_THICKNESS / 2)
    barrier.Position = Vector3.new(0, centerY, 0)
    
    barrier.Anchored = true
    barrier.CanCollide = true
    barrier.Transparency = 1          -- 完全に不透明
    barrier.BrickColor = BrickColor.new("Bright blue")
    barrier.Material = Enum.Material.Neon     -- 少し目立つように（任意）
    barrier.Parent = workspace

    -- X,Z追従
    if heartbeatConn then heartbeatConn:Disconnect() end
    heartbeatConn = RunService.Heartbeat:Connect(function()
        if not enabled or not character or not character:FindFirstChild("HumanoidRootPart") then
            if barrier then barrier:Destroy() barrier = nil end
            if heartbeatConn then heartbeatConn:Disconnect() heartbeatConn = nil end
            return
        end
        
        local hrp = character.HumanoidRootPart
        barrier.Position = Vector3.new(hrp.Position.X, centerY, hrp.Position.Z)
    end)
end

-- バリアON/OFF切り替え関数
local function toggleBarrier(state)
    enabled = state
    
    if enabled then
        if player.Character then
            updateBarrier(player.Character)
        end
        -- 通知（任意）
        -- Rayfield:Notify({Title = "Anti-Fall Barrier", Content = "有効化しました", Duration = 3})
    else
        if barrier then barrier:Destroy() barrier = nil end
        if heartbeatConn then heartbeatConn:Disconnect() heartbeatConn = nil end
        -- Rayfield:Notify({Title = "Anti-Fall Barrier", Content = "無効化しました", Duration = 3})
    end
end

-- キャラクター追加時（死亡→リスポーン含む）
player.CharacterAdded:Connect(function(char)
    if enabled then
        -- 少し待ってから作成（HumanoidRootPartが確実に存在するように）
        task.delay(0.1, function()
            updateBarrier(char)
        end)
    end
end)

-- 既にキャラクターが存在する場合
if player.Character then
    task.delay(0.1, function()
        if enabled then
            updateBarrier(player.Character)
        end
    end)
end


BloxfruitTab:CreateToggle({
    Name = "WalkWater",
    CurrentValue = false,
    Callback = function(Value)
        toggleBarrier(Value)
    end,
})

-- スクリプト終了時のクリーンアップ
script.AncestryChanged:Connect(function(_, parent)
    if parent == nil then
        if heartbeatConn then heartbeatConn:Disconnect() end
        if barrier then barrier:Destroy() end
    end
end)
BloxfruitTab:CreateToggle({
    Name = "マグマ無効",
    CurrentValue = false,
    Callback = function(Value)
        NoLavaEnabled = Value
        if Value then DisableLavaDamage() end
        Rayfield:Notify({Title = Value and "ON" or "OFF", Content = "溶岩ダメージ無効化", Duration = 3})
    end,
})



-- Autov3 トグル追加（BloxfruitTab内に挿入）
local AutoV3Enabled = false
local AutoV3Thread = nil
local RS = game:GetService("ReplicatedStorage")
local CommE = RS:WaitForChild("Remotes"):WaitForChild("CommE")

BloxfruitTab:CreateToggle({
    Name = "Autov3",
    CurrentValue = false,
    Callback = function(Value)
        AutoV3Enabled = Value
        
        if Value then
            AutoV3Thread = task.spawn(function()
                while AutoV3Enabled do
                    pcall(function()
                        CommE:FireServer("ActivateAbility")
                    end)
                    task.wait(0.1) -- 調整可能
                end
            end)
            Rayfield:Notify({
                Title = "ON",
                Content = "Autov3 開始",
                Duration = 3
            })
        else
            if AutoV3Thread then
                task.cancel(AutoV3Thread)
                AutoV3Thread = nil
            end
            Rayfield:Notify({
                Title = "OFF",
                Content = "Autov3 停止",
                Duration = 3
            })
        end
    end,
})


-- Blox Fruits: Auto Load Fruit + Auto Chip (Rayfield用・完全分離版)
-- 自動ロードと自動チップを別トグルで制御

local RS = game:GetService("ReplicatedStorage")
local CommF = RS:WaitForChild("Remotes"):WaitForChild("CommF_")

-- 全実リスト（価値低い順）
local FruitsByValue = {
    "Rocket", "Spin", "Blade", "Spring", "Bomb", "Spike", "Smoke",      -- Common
    "Flame", "Ice", "Sand", "Dark", "Eagle", "Diamond",                 -- Uncommon
    "Light", "Rubber", "Ghost", "Magma",                                -- Rare
    "Quake", "Buddha", "Love", "Creation", "Spider", "Sound", "Portal", -- Legendary
    "Lightning", "Pain", "Blizzard", "Phoenix",
    "Gravity", "Shadow", "Venom", "Dough", "Control", "Spirit", "Gas",  -- Mythical
    "Mammoth", "T-Rex", "Tiger", "Yeti", "Kitsune", "Dragon"
}

-- 選択状態
local SelectedFruits = {}           -- 複数選択された実（ロード＆チップ共通）
local AutoLoadEnabled = false       -- 自動ロード用
local AutoChipEnabled = false       -- 自動チップ用
local LoadThread = nil
local ChipThread = nil

-- インベントリ内の実名リストを取得
local function getInventoryFruitNames()
    local inv = {}
    pcall(function()
        local result = CommF:InvokeServer("getInventory")
        if result and result["Fruits"] then
            for _, fruit in ipairs(result["Fruits"]) do
                local name = fruit["Name"]:gsub(" Fruit$", "")  -- "Rocket Fruit" → "Rocket"
                table.insert(inv, name)
            end
        end
    end)
    return inv
end

-- 自動ロード処理（別スレッド）
local function startLoadThread()
    if LoadThread then task.cancel(LoadThread) end
    
    LoadThread = task.spawn(function()
        while AutoLoadEnabled do
            pcall(function()
                local inv = getInventoryFruitNames()
                local hasAnyFruit = #inv > 0
                
                -- インベントリが完全に空の場合のみロード
                if not hasAnyFruit then
                    for _, fruit in ipairs(FruitsByValue) do
                        if table.find(SelectedFruits, fruit) then
                            CommF:InvokeServer("LoadFruit", fruit .. "-" .. fruit)
                            task.wait(0.8)  -- ロード間隔（BAN対策）
                            break           -- 1回に1つだけ
                        end
                    end
                end
            end)
            task.wait(2)  -- チェック間隔
        end
    end)
end

-- 自動チップ処理（別スレッド）
local function startChipThread()
    if ChipThread then task.cancel(ChipThread) end
    
    ChipThread = task.spawn(function()
        while AutoChipEnabled do
            pcall(function()
                local inv = getInventoryFruitNames()
                
                for _, fruit in ipairs(SelectedFruits) do
                    if table.find(inv, fruit) then
                        CommF:InvokeServer("Chip", fruit)
                        task.wait(0.8)  -- チップ間隔
                    end
                end
            end)
            task.wait(2)  -- チェック間隔
        end
    end)
end

-- 複数選択ドロップダウン（共通）
BloxfruitTab:CreateDropdown({
    Name = "対象の実を選択（複数可）",
    Options = FruitsByValue,
    CurrentOption = {},
    MultipleOptions = true,
    Callback = function(option)
        SelectedFruits = option
    end,
})

-- 自動ロード専用トグル
BloxfruitTab:CreateToggle({
    Name = "自動ロード（インベントリ空時のみ）",
    CurrentValue = false,
    Callback = function(Value)
        AutoLoadEnabled = Value
        
        if Value then
            if #SelectedFruits == 0 then
                Rayfield:Notify({
                    Title = "注意",
                    Content = "少なくとも1つの実を選択してください",
                    Duration = 4
                })
                AutoLoadEnabled = false
                return
            end
            startLoadThread()
            Rayfield:Notify({
                Title = "自動ロード開始",
                Content = "インベントリが空の時、選択実を低価値順にロード",
                Duration = 5
            })
        else
            if LoadThread then
                task.cancel(LoadThread)
                LoadThread = nil
            end
            Rayfield:Notify({
                Title = "自動ロード停止",
                Content = "ロード自動化を停止しました",
                Duration = 4
            })
        end
    end,
})

-- 自動チップ専用トグル
BloxfruitTab:CreateToggle({
    Name = "自動チップ（選択実のみ）",
    CurrentValue = false,
    Callback = function(Value)
        AutoChipEnabled = Value
        
        if Value then
            if #SelectedFruits == 0 then
                Rayfield:Notify({
                    Title = "注意",
                    Content = "少なくとも1つの実を選択してください",
                    Duration = 4
                })
                AutoChipEnabled = false
                return
            end
            startChipThread()
            Rayfield:Notify({
                Title = "自動チップ開始",
                Content = "選択した実を自動でチップ化（持ってる場合）",
                Duration = 5
            })
        else
            if ChipThread then
                task.cancel(ChipThread)
                ChipThread = nil
            end
            Rayfield:Notify({
                Title = "自動チップ停止",
                Content = "チップ自動化を停止しました",
                Duration = 4
            })
        end
    end,
})


-- TP座標
local locations = {
    ["マンション"] = Vector3.new(-12549, 337, -7506),
    ["Tiki"] = Vector3.new(-5093, 316, -3182),
    ["海城"] = Vector3.new(-12463, 376, -7566),
    ["ヒドラ"] = Vector3.new(-5027, 316, -3206)
}

-- 超安定ダブルTP関数（高速2回TP）
local function doubleTp(pos)
    local char = player.Character
    if not char or not char:FindFirstChild("HumanoidRootPart") then return end
    local root = char.HumanoidRootPart
    
    -- 共通TP処理
    local function performTp()
        pcall(function() root:SetNetworkOwner(player) end)  -- 所有権奪取
        
        -- 毎回違う微ランダムオフセット（検知回避）
        local offset = Vector3.new(math.random(-4,4), math.random(6,16), math.random(-4,4))
        root.CFrame = CFrame.new(pos + offset)
        
        -- 速度リセット（Kick回避）
        if char:FindFirstChild("Humanoid") then
            char.Humanoid.WalkSpeed = 16
            char.Humanoid.JumpPower = 50
        end
    end
    
    -- 1回目TP
    performTp()
    
    -- 高速0.05秒待機 → 2回目TP（ほぼ瞬時に2回）
    task.delay(0.05, function()
        performTp()
    end)
end

-- Noclip常時ON（壁抜け・張り付き安定）
local noclip = false
local function toggleNoclip(state)
    noclip = state
    local char = player.Character
    if not char then return end
    for _, part in pairs(char:GetDescendants()) do
        if part:IsA("BasePart") and part ~= char.HumanoidRootPart then
            part.CanCollide = not state
        end
    end
end
task.spawn(function()
    toggleNoclip(true)
    while task.wait(0.1) do
        if noclip then toggleNoclip(true) end
    end
end)

-- 自作TP GUI作成（初期非表示・トグルで表示）
local tpGui = Instance.new("ScreenGui")
tpGui.Name = "TPHub"
tpGui.ResetOnSpawn = false
tpGui.Enabled = false
tpGui.Parent = player:WaitForChild("PlayerGui")

local mainFrame = Instance.new("Frame")
mainFrame.Size = UDim2.new(0, 280, 0, 350)
mainFrame.Position = UDim2.new(0.5, -140, 0.5, -175)
mainFrame.BackgroundColor3 = Color3.fromRGB(25, 25, 35)
mainFrame.BorderSizePixel = 0
mainFrame.Parent = tpGui

local uicorner = Instance.new("UICorner")
uicorner.CornerRadius = UDim.new(0, 12)
uicorner.Parent = mainFrame

local uigradient = Instance.new("UIGradient")
uigradient.Color = ColorSequence.new{
    ColorSequenceKeypoint.new(0, Color3.fromRGB(40, 40, 60)),
    ColorSequenceKeypoint.new(1, Color3.fromRGB(20, 20, 35))
}
uigradient.Rotation = 45
uigradient.Parent = mainFrame

-- タイトル（ドラッグ可能）
local title = Instance.new("TextLabel")
title.Size = UDim2.new(1, 0, 0, 50)
title.BackgroundTransparency = 1
title.Text = "TP Hub"
title.TextColor3 = Color3.fromRGB(255, 255, 255)
title.TextScaled = true
title.Font = Enum.Font.GothamBold
title.Parent = mainFrame

-- ドラッグ機能
local dragging = false
local dragStart, startPos
title.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = true
        dragStart = input.Position
        startPos = mainFrame.Position
    end
end)
title.InputChanged:Connect(function(input)
    if dragging and input.UserInputType == Enum.UserInputType.MouseMovement then
        local delta = input.Position - dragStart
        mainFrame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
    end
end)
game:GetService("UserInputService").InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = false
    end
end)

-- ボタン生成関数
local function createTPButton(name, yPos, posVec)
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(1, -20, 0, 55)
    btn.Position = UDim2.new(0, 10, 0, yPos)
    btn.BackgroundColor3 = Color3.fromRGB(50, 50, 70)
    btn.Text = "⚡ " .. name
    btn.TextColor3 = Color3.fromRGB(255, 255, 255)
    btn.TextScaled = true
    btn.Font = Enum.Font.GothamSemibold
    btn.Parent = mainFrame
    
    local btnCorner = Instance.new("UICorner")
    btnCorner.CornerRadius = UDim.new(0, 10)
    btnCorner.Parent = btn
    
    local btnGradient = Instance.new("UIGradient")
    btnGradient.Color = ColorSequence.new{
        ColorSequenceKeypoint.new(0, Color3.fromRGB(70, 70, 90)),
        ColorSequenceKeypoint.new(1, Color3.fromRGB(40, 40, 60))
    }
    btnGradient.Parent = btn
    
    -- ホバーアニメ
    btn.MouseEnter:Connect(function()
        TweenService:Create(btn, TweenInfo.new(0.2), {Size = UDim2.new(1, -15, 0, 58), BackgroundColor3 = Color3.fromRGB(70, 130, 255)}):Play()
    end)
    btn.MouseLeave:Connect(function()
        TweenService:Create(btn, TweenInfo.new(0.2), {Size = UDim2.new(1, -20, 0, 55), BackgroundColor3 = Color3.fromRGB(50, 50, 70)}):Play()
    end)
    
    -- TP実行（ダブルTP）
    btn.MouseButton1Click:Connect(function()
        TweenService:Create(btn, TweenInfo.new(0.1), {BackgroundColor3 = Color3.fromRGB(0, 255, 0)}):Play()
        task.wait(0.1)
        TweenService:Create(btn, TweenInfo.new(0.2), {BackgroundColor3 = Color3.fromRGB(50, 50, 70)}):Play()
        doubleTp(posVec)
    end)
end

-- ボタン配置
createTPButton("マンション", 65, locations["マンション"])
createTPButton("Tiki", 130, locations["Tiki"])
createTPButton("海城", 195, locations["海城"])
createTPButton("ヒドラ", 260, locations["ヒドラ"])


BloxfruitTab:CreateButton({
    Name = "⚡ マンション",
    Callback = function()
        doubleTp(locations["マンション"])
    end,
})
BloxfruitTab:CreateButton({
    Name = "⚡ Tiki",
    Callback = function()
        doubleTp(locations["Tiki"])
    end,
})
BloxfruitTab:CreateButton({
    Name = "⚡ 海城",
    Callback = function()
        doubleTp(locations["海城"])
    end,
})
BloxfruitTab:CreateButton({
    Name = "⚡ ヒドラ",
    Callback = function()
        doubleTp(locations["ヒドラ"])
    end,
})


--━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
-- Tab: スタンドの世界
--━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
local StandTab = Window:CreateTab("スタンドの世界", 4483362458)
-- ── キャラクター参照（Heartbeatで常時更新） ────────
local StandHumanoid, StandRootPart, StandParts = nil, nil, {}
local function UpdateStandCharacter()
    local char = LocalPlayer.Character
    if not char or not char.Parent then
        StandHumanoid = nil
        StandRootPart = nil
        StandParts = {}
        return
    end
   
    StandHumanoid = char:FindFirstChildOfClass("Humanoid")
    StandRootPart = char:FindFirstChild("HumanoidRootPart")
   
    if not StandHumanoid or not StandRootPart then return end
   
    StandParts = {}
    for _, v in ipairs(char:GetDescendants()) do
        if v:IsA("BasePart") then
            table.insert(StandParts, v)
        end
    end
end
-- 初回
UpdateStandCharacter()
-- 毎フレーム監視（リスポーン対応）
RunService.Heartbeat:Connect(UpdateStandCharacter)
-- ── Chest System ────────────────────────────────────
local CurrentChest = 0
local MaxChest = 54
local AvailableChests = {}
for i = 1, MaxChest do
    table.insert(AvailableChests, tostring(i))
end
local ChestLabel = StandTab:CreateLabel("現在のチェスト: 0")
-- Dropdown
local ChestDropdown = StandTab:CreateDropdown({
    Name = "開くチェストを選択",
    Options = AvailableChests,
    CurrentOption = {AvailableChests[1]},
    MultipleOptions = false,
    Callback = function(option)
        local num = tonumber(option[1])
        if not num then return end
       
        local chest = Workspace:FindFirstChild(tostring(num))
        if chest and chest.PrimaryPart and StandRootPart then
            -- Invisible OFF
            -- ※ setInvisible(false) は後述の関数に置き換え可能
            for _, part in ipairs(StandParts) do
                pcall(function() part.Transparency = 0 end)
            end
           
            StandRootPart.CFrame = CFrame.new(chest.PrimaryPart.Position + Vector3.new(0, 7, 0))
            CurrentChest = num
            ChestLabel:Set("現在のチェスト: " .. num)
        end
    end,
})
-- Input
StandTab:CreateInput({
    Name = "チェスト番号入力",
    PlaceholderText = "1〜" .. MaxChest,
    RemoveTextAfterFocusLost = false,
    Callback = function(text)
        local num = tonumber(text)
        if not num or num < 1 or num > MaxChest then return end
       
        local chest = Workspace:FindFirstChild(tostring(num))
        if chest and chest.PrimaryPart and StandRootPart then
            for _, part in ipairs(StandParts) do
                pcall(function() part.Transparency = 0 end)
            end
           
            StandRootPart.CFrame = CFrame.new(chest.PrimaryPart.Position + Vector3.new(0, 7, 0))
            CurrentChest = num
            ChestLabel:Set("現在のチェスト: " .. num)
        end
    end,
})
-- Next Button
StandTab:CreateButton({
    Name = "次のチェストにTP",
    Callback = function()
        CurrentChest = CurrentChest + 1
        if CurrentChest > MaxChest then CurrentChest = 1 end
       
        local chest = Workspace:FindFirstChild(tostring(CurrentChest))
        if chest and chest.PrimaryPart and StandRootPart then
            for _, part in ipairs(StandParts) do
                pcall(function() part.Transparency = 0 end)
            end
           
            StandRootPart.CFrame = CFrame.new(chest.PrimaryPart.Position + Vector3.new(0, 7, 0))
            ChestLabel:Set("現在のチェスト: " .. CurrentChest)
        end
    end,
})
-- チェスト自動更新（RenderStepped → Heartbeatに変更で軽量化）
RunService.Heartbeat:Connect(function()
    local changed = false
    for i = #AvailableChests, 1, -1 do
        if not Workspace:FindFirstChild(AvailableChests[i]) then
            table.remove(AvailableChests, i)
            changed = true
        end
    end
   
    if changed then
        ChestDropdown:Refresh(AvailableChests)
    end
end)
--━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
-- Tab: 戦闘(BloxFruit用)
--━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
local AutoAimTab = Window:CreateTab("戦闘(BloxFruit用)", 4483362458)
-- ── Auto Aim ────────────────────────────────────────
local AutoAimEnabled = false
local LockedPart = nil
local FOV_RADIUS = 160
local AIM_PART = "HumanoidRootPart"
local AIM_STRENGTH = 0.35
local ShowFOV = true
local FOVCircle = Drawing.new("Circle")
FOVCircle.Radius = FOV_RADIUS
FOVCircle.Thickness = 2
FOVCircle.NumSides = 64
FOVCircle.Filled = false
FOVCircle.Color = Color3.fromRGB(255, 255, 255)
FOVCircle.Visible = false
local function IsShiftLocked()
    return UserInputService.MouseBehavior == Enum.MouseBehavior.LockCenter
end
local function GetClosestPlayerInFOV()
    local closest, shortest = nil, math.huge
    local center = Vector2.new(Camera.ViewportSize.X / 2, Camera.ViewportSize.Y / 2)
   
    for _, plr in ipairs(Players:GetPlayers()) do
        if plr == LocalPlayer or not plr.Character then continue end
       
        local hum = plr.Character:FindFirstChildOfClass("Humanoid")
        local part = plr.Character:FindFirstChild(AIM_PART)
       
        if hum and hum.Health > 0 and part then
            local pos, onScreen = Camera:WorldToViewportPoint(part.Position)
            if onScreen then
                local dist = (Vector2.new(pos.X, pos.Y) - center).Magnitude
                if dist < FOV_RADIUS and dist < shortest then
                    shortest = dist
                    closest = part
                end
            end
        end
    end
    return closest
end
RunService.RenderStepped:Connect(function()
    if not AutoAimEnabled then
        LockedPart = nil
        FOVCircle.Visible = false
        return
    end
   
    -- FOV描画
    FOVCircle.Position = Vector2.new(Camera.ViewportSize.X / 2, Camera.ViewportSize.Y / 2)
    FOVCircle.Radius = FOV_RADIUS
    FOVCircle.Visible = ShowFOV
   
    if not IsShiftLocked() then
        LockedPart = nil
        return
    end
   
    if not LockedPart or not LockedPart.Parent then
        LockedPart = GetClosestPlayerInFOV()
    end
   
    if LockedPart then
        local camCF = Camera.CFrame
        local targetCF = CFrame.new(camCF.Position, LockedPart.Position)
        Camera.CFrame = camCF:Lerp(targetCF, AIM_STRENGTH)
    end
end)
-- ── Fruit Auto Collect ──────────────────────────────
local FruitSlideEnabled = false
local FruitTPSlideEnabled = false
local SlideSpeed = 300
local HeightOffset = 0
local FruitCheckInterval = 0.2
local function GetRootPart()
    local char = LocalPlayer.Character
    return char and char:FindFirstChild("HumanoidRootPart")
end
local function GetNearestFruit()
    local root = GetRootPart()
    if not root then return nil end
   
    local closest, minDist = nil, math.huge
    for _, obj in ipairs(Workspace:GetDescendants()) do
        if obj:IsA("BasePart") and obj.Name == "Fruit" then
            local dist = (obj.Position - root.Position).Magnitude
            if dist < minDist then
                minDist = dist
                closest = obj
            end
        end
    end
    return closest
end
-- スライド回収
RunService.RenderStepped:Connect(function(dt)
    if not FruitSlideEnabled then return end
   
    local root = GetRootPart()
    if not root then return end
   
    local fruit = GetNearestFruit()
    if not fruit then return end
   
    root.AssemblyLinearVelocity = Vector3.zero
   
    local targetPos = Vector3.new(fruit.Position.X, root.Position.Y + HeightOffset, fruit.Position.Z)
    local dir = targetPos - root.Position
   
    if dir.Magnitude < 2 then return end
   
    root.CFrame += dir.Unit * SlideSpeed * dt
end)
-- 瞬間TP回収
task.spawn(function()
    while true do
        task.wait(FruitCheckInterval)
        if not FruitTPSlideEnabled then continue end
       
        local root = GetRootPart()
        if not root then continue end
       
        local fruit = GetNearestFruit()
        if not fruit then continue end
       
        local originalCF = root.CFrame
        root.CFrame = fruit.CFrame
        task.wait(0.05)
        root.CFrame = originalCF
    end
end)
-- GUI
AutoAimTab:CreateToggle({
    Name = "オートエイム",
    CurrentValue = false,
    Callback = function(v) AutoAimEnabled = v end,
})
AutoAimTab:CreateToggle({
    Name = "FOV 表示",
    CurrentValue = true,
    Callback = function(v) ShowFOV = v end,
})
AutoAimTab:CreateSlider({
    Name = "FOV 大きさ",
    Range = {50, 400},
    Increment = 5,
    Suffix = "px",
    CurrentValue = FOV_RADIUS,
    Callback = function(v) FOV_RADIUS = v end,
})
AutoAimTab:CreateSlider({
    Name = "吸い付き強度",
    Range = {0.1, 1},
    Increment = 0.05,
    CurrentValue = AIM_STRENGTH,
    Callback = function(v) AIM_STRENGTH = v end,
})
AutoAimTab:CreateToggle({
    Name = "Fruit 自動回収（スライド）",
    CurrentValue = false,
    Callback = function(v) FruitSlideEnabled = v end,
})
AutoAimTab:CreateToggle({
    Name = "Fruit 瞬間回収（TP）",
    CurrentValue = false,
    Callback = function(v) FruitTPSlideEnabled = v end,
})
-- Fruit一覧（簡易版）
local FruitList = {}
local FruitDropdown = AutoAimTab:CreateDropdown({
    Name = "Fruit 一覧",
    Options = FruitList,
    Callback = function(selected)
        print("選択:", selected)
    end,
})
AutoAimTab:CreateButton({
    Name = "Fruitリスト更新",
    Callback = function()
        FruitList = {}
        for _, obj in ipairs(Workspace:GetDescendants()) do
            if obj:IsA("BasePart") and obj.Name == "Fruit" then
                table.insert(FruitList, obj:GetFullName())
            end
        end
        FruitDropdown:Refresh(FruitList)
    end,
})
-- Head Hitbox
local HeadHitboxEnabled = false
local HeadScale = 1
local OriginalHeadSizes = {}
AutoAimTab:CreateToggle({
    Name = "Head拡大（全員）",
    CurrentValue = false,
    Callback = function(v)
        HeadHitboxEnabled = v
        for _, plr in ipairs(Players:GetPlayers()) do
            if plr == LocalPlayer then continue end
            local head = plr.Character and plr.Character:FindFirstChild("Head")
            if head and head:IsA("BasePart") then
                if v then
                    if not OriginalHeadSizes[head] then
                        OriginalHeadSizes[head] = head.Size
                    end
                    head.Size = OriginalHeadSizes[head] * HeadScale
                    head.Transparency = 0.5
                    head.CanCollide = false
                    head.Massless = true
                else
                    if OriginalHeadSizes[head] then
                        head.Size = OriginalHeadSizes[head]
                    end
                    head.Transparency = 0
                end
            end
        end
    end,
})
AutoAimTab:CreateSlider({
    Name = "Head倍率",
    Range = {1, 2000},
    Increment = 0.1,
    Suffix = "倍",
    CurrentValue = HeadScale,
    Callback = function(v)
        HeadScale = v
        if HeadHitboxEnabled then
            for head, orig in pairs(OriginalHeadSizes) do
                if head and head.Parent then
                    head.Size = orig * v
                    head.Transparency = 0.5
                end
            end
        end
    end,
})
--━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
-- Tab: 敵処理
--━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
local EnemyTab = Window:CreateTab("敵処理", 4483362458)
local FollowDistance = 4
local AttractionRadius = 1
EnemyTab:CreateSlider({
    Name = "敵の前方距離",
    Range = {1, 80},
    Increment = 1,
    Suffix = " studs",
    CurrentValue = FollowDistance,
    Callback = function(v) FollowDistance = v end,
})
EnemyTab:CreateSlider({
    Name = "吸引半径",
    Range = {1, 2000},
    Increment = 1,
    Suffix = " studs",
    CurrentValue = AttractionRadius,
    Callback = function(v) AttractionRadius = v end,
})
-- 敵吸引処理（毎フレーム）
RunService.RenderStepped:Connect(function()
    local char = LocalPlayer.Character
    if not char then return end
   
    local hrp = char:FindFirstChild("HumanoidRootPart")
    if not hrp then return end
   
    local enemies = Workspace:FindFirstChild("Enemies")
    if not enemies then return end
   
    for _, enemy in ipairs(enemies:GetChildren()) do
        local eHRP = enemy:FindFirstChild("HumanoidRootPart")
        if eHRP then
            local dist = (eHRP.Position - hrp.Position).Magnitude
            if dist <= AttractionRadius then
                eHRP.CFrame = hrp.CFrame * CFrame.new(0, 0, -FollowDistance)
            end
        end
    end
end)
--━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
-- Tab: Server（サーバー管理）
--━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
local ServerTab = Window:CreateTab("Server", 4483362458)
-- ── 状態管理 ────────────────────────────────────────
local ServerSettings = {
    AutoRejoinEnabled = false,
    RejoinHotkeyEnabled = false,
    RejoinKey = Enum.KeyCode.F6,
}
local LastPrivateServerCode = nil
local PrivateServerLink = ""
-- ── Ping表示 ────────────────────────────────────────
local PingLabel = ServerTab:CreateLabel("Ping: -- ms")
RunService.RenderStepped:Connect(function()
    local pingItem = Stats.Network.ServerStatsItem:FindFirstChild("Data Ping")
    if pingItem then
        local ping = math.floor(pingItem:GetValue())
        PingLabel:Set("Ping: " .. ping .. " ms")
    else
        PingLabel:Set("Ping: N/A")
    end
end)
-- ── サーバー入り直し ────────────────────────────────
ServerTab:CreateButton({
    Name = "サーバー入りなおし",
    Callback = function()
        TeleportService:Teleport(game.PlaceId, LocalPlayer)
    end,
})
-- ── ホットキー設定 ──────────────────────────────────
ServerTab:CreateToggle({
    Name = "入りなおしキー (F6)",
    CurrentValue = false,
    Callback = function(v)
        ServerSettings.RejoinHotkeyEnabled = v
    end,
})
UserInputService.InputBegan:Connect(function(input, gameProcessed)
    if gameProcessed then return end
    if ServerSettings.RejoinHotkeyEnabled and input.KeyCode == ServerSettings.RejoinKey then
        TeleportService:Teleport(game.PlaceId, LocalPlayer)
    end
end)
-- ── プライベートサーバー参加 ────────────────────────
ServerTab:CreateInput({
    Name = "プライベートサーバーリンク",
    PlaceholderText = "...privateServerLinkCode=XXXX",
    RemoveTextAfterFocusLost = false,
    Callback = function(text)
        PrivateServerLink = text
    end,
})
ServerTab:CreateButton({
    Name = "プライベートサーバーに参加 (リンク必須)",
    Callback = function()
        if PrivateServerLink == "" then return end
       
        local code = PrivateServerLink:match("privateServerLinkCode=([%w%-]+)")
        if not code or code == "" then
            Rayfield:Notify({
                Title = "エラー",
                Content = "有効なプライベートサーバーコードが見つかりません",
                Duration = 4
            })
            return
        end
       
        LastPrivateServerCode = code
       
        pcall(function()
            TeleportService:TeleportToPrivateServer(
                game.PlaceId,
                code,
                { LocalPlayer }
            )
        end)
    end,
})
-- ── ランダムサーバー参加 ────────────────────────────
ServerTab:CreateButton({
    Name = "サーバーに参加 (ランダム)",
    Callback = function()
        TeleportService:Teleport(game.PlaceId, LocalPlayer)
    end,
})
-- ── 空きサーバー参加 ────────────────────────────────
ServerTab:CreateButton({
    Name = "人数が少ないサーバーに参加",
    Callback = function()
        local servers = {}
        local cursor = ""
       
        repeat
            local url = string.format(
                "https://games.roblox.com/v1/games/%d/servers/Public?sortOrder=Asc&limit=100%s",
                game.PlaceId,
                cursor ~= "" and "&cursor=" .. cursor or ""
            )
           
            local success, response = pcall(function()
                return HttpService:JSONDecode(game:HttpGet(url))
            end)
           
            if not success or not response or not response.data then
                Rayfield:Notify({Title="エラー", Content="サーバーリスト取得に失敗", Duration=4})
                return
            end
           
            for _, server in ipairs(response.data) do
                if server.playing < server.maxPlayers and server.id ~= game.JobId then
                    table.insert(servers, server.id)
                end
            end
           
            cursor = response.nextPageCursor or ""
        until #servers > 0 or cursor == ""
       
        if #servers > 0 then
            local randomId = servers[math.random(1, #servers)]
            TeleportService:TeleportToPlaceInstance(game.PlaceId, randomId, LocalPlayer)
        else
            Rayfield:Notify({Title="情報", Content="空きサーバーが見つかりませんでした", Duration=4})
        end
    end,
})
-- ── フレンドのいるサーバー参加 ──────────────────────
ServerTab:CreateButton({
    Name = "フレンドのいるサーバーに参加",
    Callback = function()
        local success, pages = pcall(function()
            return Players:GetFriendsAsync(LocalPlayer.UserId)
        end)
       
        if not success then
            Rayfield:Notify({Title="エラー", Content="フレンドリスト取得に失敗", Duration=4})
            return
        end
       
        for _, friend in ipairs(pages:GetCurrentPage()) do
            if friend.IsOnline and friend.PlaceId == game.PlaceId then
                TeleportService:TeleportToPlaceInstance(
                    game.PlaceId,
                    friend.GameId,
                    LocalPlayer
                )
                Rayfield:Notify({
                    Title = "参加中",
                    Content = "フレンドのいるサーバーに移動します",
                    Duration = 4
                })
                return
            end
        end
       
        Rayfield:Notify({Title="情報", Content="オンラインのフレンドが見つかりませんでした", Duration=4})
    end,
})
-- ── キック時自動再参加 ──────────────────────────────
ServerTab:CreateToggle({
    Name = "キック時に自動参加",
    CurrentValue = false,
    Callback = function(v)
        ServerSettings.AutoRejoinEnabled = v
    end,
})
GuiService.ErrorMessageChanged:Connect(function(errorMsg)
    if not ServerSettings.AutoRejoinEnabled then return end
   
    -- エラーメッセージにキック関連の文字列が含まれる場合のみ反応
    if errorMsg and (errorMsg:find("kick") or errorMsg:find("banned") or errorMsg:find("disconnect")) then
        task.wait(3) -- 少し長めに待機して安定
       
        if LastPrivateServerCode then
            pcall(function()
                TeleportService:TeleportToPrivateServer(
                    game.PlaceId,
                    LastPrivateServerCode,
                    { LocalPlayer }
                )
            end)
        else
            TeleportService:Teleport(game.PlaceId, LocalPlayer)
        end
    end
end)
