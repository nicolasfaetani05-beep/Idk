-- Script Optimizado: Muerte Real ಠ_ಠ
local Player = game.Players.LocalPlayer
local Character = Player.Character or Player.CharacterAdded:Wait()
local Hum = Character:WaitForChild("Humanoid")
local Root = Character:WaitForChild("HumanoidRootPart")

-- Audios ಠ_ಠ
local SoundAttack = game:GetService("SoundService"):FindFirstChild("Twisted_Squirm_Attack.mp3.mpeg.mp3", true)
local SoundEscape = game:GetService("SoundService"):FindFirstChild("Twisted_Squirm_Player_Escape.mp3.mpeg.mp3")

-- Interfaz ✯⁠ᴗ⁠✯
local ScreenGui = Instance.new("ScreenGui", Player.PlayerGui)
ScreenGui.Name = "SquirmMinigame"
local MainFrame = Instance.new("Frame", ScreenGui)
MainFrame.Size = UDim2.new(0, 450, 0, 250)
MainFrame.Position = UDim2.new(0.5, -225, 0.4, 0)
MainFrame.BackgroundTransparency = 1

-- --- BARRA ROJA (TIEMPO) ಠ_ಠ ---
local TimeBarBack = Instance.new("Frame", MainFrame)
TimeBarBack.Size = UDim2.new(0.8, 0, 0.1, 0)
TimeBarBack.Position = UDim2.new(0.1, 0, 0, 0)
TimeBarBack.BackgroundColor3 = Color3.fromRGB(40, 0, 0)

local ClockLabel = Instance.new("TextLabel", TimeBarBack)
ClockLabel.Size = UDim2.new(0.2, 0, 1.5, 0)
ClockLabel.Position = UDim2.new(0.4, 0, -0.25, 0)
ClockLabel.Text = "⌚"
ClockLabel.BackgroundTransparency = 1
ClockLabel.TextScaled = true

local RedLeft = Instance.new("Frame", TimeBarBack)
RedLeft.Size = UDim2.new(0.5, 0, 1, 0)
RedLeft.BackgroundColor3 = Color3.fromRGB(255, 0, 0)

local RedRight = Instance.new("Frame", TimeBarBack)
RedRight.Size = UDim2.new(0.5, 0, 1, 0)
RedRight.Position = UDim2.new(0.5, 0, 0, 0)
RedRight.BackgroundColor3 = Color3.fromRGB(255, 0, 0)

-- --- BARRA VERDE (PROGRESO) ಠ_ಠ ---
local BarBack = Instance.new("Frame", MainFrame)
BarBack.Size = UDim2.new(0.8, 0, 0.15, 0)
BarBack.Position = UDim2.new(0.1, 0, 0.2, 0)
BarBack.BackgroundColor3 = Color3.fromRGB(30, 30, 30)

local BarFill = Instance.new("Frame", BarBack)
BarFill.Size = UDim2.new(0, 0, 1, 0)
BarFill.BackgroundColor3 = Color3.fromRGB(0, 255, 0)

-- --- BOTONES ✯⁠ᴗ⁠✯ ---
local function CreateBtn(text, pos)
    local b = Instance.new("TextButton", MainFrame)
    b.Size = UDim2.new(0, 120, 0, 100)
    b.Position = pos
    b.Text = text
    b.BackgroundColor3 = Color3.fromRGB(80, 80, 80)
    b.TextColor3 = Color3.new(1, 1, 1)
    b.TextScaled = true
    return b
end

local LeftBtn = CreateBtn("<\nTAP", UDim2.new(0.1, 0, 0.5, 0))
local RightBtn = CreateBtn(">\nTAP", UDim2.new(0.65, 0, 0.5, 0))

-- --- LÓGICA DE PROCESAMIENTO ಠ_ಠ ---
local progress = 0
local isEscaped = false
local lastClicked = ""
local timeScale = 1 

if SoundAttack then SoundAttack.Looped = true SoundAttack:Play() end
Hum.WalkSpeed = 0
Hum.JumpPower = 0

task.spawn(function()
    local totalTime = 6
    while timeScale > 0 and not isEscaped do
        local dt = task.wait(0.05)
        timeScale = timeScale - (dt / totalTime)
        RedLeft.Size = UDim2.new(math.max(0, timeScale/2), 0, 1, 0)
        RedRight.Size = UDim2.new(math.max(0, timeScale/2), 0, 1, 0)
        RedRight.Position = UDim2.new(1 - (timeScale/2), 0, 0, 0)
    end
    
    if not isEscaped then
        task.wait(2) -- 2s de gracia ಠ_ಠ
        if not isEscaped then
            if SoundAttack then SoundAttack:Stop() end
            -- MATAR Y MANDAR AL VACÍO (SIN RETORNO) ಠ_ಠ
            Hum.Health = 0 -- Se asegura de que muera
            Root.CFrame = CFrame.new(Root.Position.X, -5000, Root.Position.Z)
            ScreenGui:Destroy()
        end
    end
end)

local function OnClick(side)
    if isEscaped then return end
    if side ~= lastClicked then
        progress = math.min(progress + 5, 100)
        BarFill.Size = UDim2.new(progress/100, 0, 1, 0)
        lastClicked = side
        if progress >= 100 then
            isEscaped = true
            if SoundAttack then SoundAttack:Stop() end
            if SoundEscape then SoundEscape:Play() end
            Hum.WalkSpeed = 16
            Hum.JumpPower = 50
            Root.CFrame = Root.CFrame * CFrame.new(0, 0, 10)
            ScreenGui:Destroy()
        end
    end
end

LeftBtn.MouseButton1Click:Connect(function() OnClick("Left") end)
RightBtn.MouseButton1Click:Connect(function() OnClick("Right") end)
