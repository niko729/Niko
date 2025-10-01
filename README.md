local Players = game:GetService("Players")
local player = Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoidRootPart = character:WaitForChild("HumanoidRootPart")

local savedPos = nil

-- Crear ScreenGui
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "NIKTFC_HUB"
screenGui.ResetOnSpawn = false -- Para que no se borre al morir
screenGui.Parent = player:WaitForChild("PlayerGui")

-- Crear Frame
local frame = Instance.new("Frame")
frame.Size = UDim2.new(0, 220, 0, 180)
frame.Position = UDim2.new(0.05, 0, 0.25, 0)
frame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
frame.BorderSizePixel = 0
frame.Parent = screenGui

-- Título
local title = Instance.new("TextLabel")
title.Size = UDim2.new(1, 0, 0, 40)
title.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
title.Text = "NIKTFC Hub"
title.TextColor3 = Color3.fromRGB(255, 255, 255)
title.Font = Enum.Font.SourceSansBold
title.TextSize = 22
title.Parent = frame

-- Función para crear botones
local function createButton(name, posY, text, callback)
    local button = Instance.new("TextButton")
    button.Name = name
    button.Size = UDim2.new(1, -10, 0, 40)
    button.Position = UDim2.new(0, 5, 0, posY)
    button.BackgroundColor3 = Color3.fromRGB(70, 70, 70)
    button.TextColor3 = Color3.fromRGB(255, 255, 255)
    button.Font = Enum.Font.SourceSansBold
    button.TextSize = 18
    button.Text = text
    button.Parent = frame

    button.MouseButton1Click:Connect(callback)
end

-- Botón TP (guardar ubicación)
createButton("TP", 45, "Guardar Ubicación", function()
    savedPos = humanoidRootPart.CFrame
    warn("Ubicación guardada")
end)

-- Botón TP2 (teletransportar)
createButton("TP2", 90, "Ir a Ubicación", function()
    if savedPos then
        humanoidRootPart.CFrame = savedPos
    else
        warn("Primero guarda una ubicación con TP")
    end
end)

-- Botón TRAS (traspasar paredes)
createButton("TRAS", 135, "Traspasar Pared", function()
    for _, part in pairs(character:GetDescendants()) do
        if part:IsA("BasePart") then
            part.CanCollide = false
        end
    end
    task.wait(3) -- 3 segundos
    for _, part in pairs(character:GetDescendants()) do
        if part:IsA("BasePart") then
            part.CanCollide = true
        end
    end
end)
