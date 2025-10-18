-- Script de Teste para Asylum Life (APENAS PARA SEU SERVIDOR)
local Players = game:GetService("Players")
local localPlayer = Players.LocalPlayer

-- Funções de teste
local TestCheats = {}

-- Speed Hack
function TestCheats:SpeedHack(speed)
    local character = localPlayer.Character
    if character then
        local humanoid = character:FindFirstChildOfClass("Humanoid")
        if humanoid then
            humanoid.WalkSpeed = speed or 50
        end
    end
end

-- Fly Hack
function TestCheats:FlyHack()
    local character = localPlayer.Character
    if not character then return end
    
    local humanoid = character:FindFirstChildOfClass("Humanoid")
    local rootPart = character:FindFirstChild("HumanoidRootPart")
    
    if humanoid and rootPart then
        local bodyVelocity = Instance.new("BodyVelocity")
        bodyVelocity.Velocity = Vector3.new(0, 50, 0)
        bodyVelocity.MaxForce = Vector3.new(0, math.huge, 0)
        bodyVelocity.Parent = rootPart
        
        task.wait(2)
        bodyVelocity:Destroy()
    end
end

-- NoClip
function TestCheats:NoClip()
    local character = localPlayer.Character
    if character then
        for _, part in pairs(character:GetDescendants()) do
            if part:IsA("BasePart") then
                part.CanCollide = false
            end
        end
    end
end

-- Teleport para jogadores
function TestCheats:TeleportToPlayer(playerName)
    local target = Players:FindFirstChild(playerName)
    if target and target.Character and target.Character:FindFirstChild("HumanoidRootPart") then
        localPlayer.Character:SetPrimaryPartCFrame(target.Character.HumanoidRootPart.CFrame)
    end
end

-- Spam de mensagens (teste de chat filter)
function TestCheats:SpamChat()
    for i = 1, 10 do
        game:GetService("ReplicatedStorage"):FindFirstChild("DefaultChatSystemChatEvents"):FindFirstChild("SayMessageRequest"):FireServer(`Mensagem de spam {i}`, "All")
        task.wait(0.1)
    end
end

-- Interface de teste
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Parent = game:GetService("CoreGui")

local Frame = Instance.new("Frame")
Frame.Size = UDim2.new(0, 200, 0, 300)
Frame.Position = UDim2.new(0, 10, 0, 10)
Frame.BackgroundColor3 = Color3.new(0, 0, 0)
Frame.BackgroundTransparency = 0.3
Frame.Parent = ScreenGui

local function CreateButton(text, yPos, callback)
    local button = Instance.new("TextButton")
    button.Size = UDim2.new(0, 180, 0, 30)
    button.Position = UDim2.new(0, 10, 0, yPos)
    button.Text = text
    button.BackgroundColor3 = Color3.new(0.2, 0.2, 0.2)
    button.TextColor3 = Color3.new(1, 1, 1)
    button.Parent = Frame
    
    button.MouseButtonClick:Connect(callback)
end

-- Botões de teste
CreateButton("Speed Hack (50)", 10, function()
    TestCheats:SpeedHack(50)
end)

CreateButton("Fly Hack", 50, function()
    TestCheats:FlyHack()
end)

CreateButton("NoClip", 90, function()
    TestCheats:NoClip()
end)

CreateButton("Spam Chat", 130, function()
    TestCheats:SpamChat()
end)

CreateButton("TP para Spawn", 170, function()
    localPlayer.Character:SetPrimaryPartCFrame(CFrame.new(0, 10, 0))
end)

warn("Script de teste carregado! Use apenas no seu próprio servidor.")
