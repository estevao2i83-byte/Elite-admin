local VALID_KEY = "7f9c2a81-b4d3-4e6f-9a2c-5d8e1b7c3f90"
local DISCORD_LINK = "https://discord.gg/nQ3YqHFPCw"

local Players = game:GetService("Players")
local player = Players.LocalPlayer
local pgui = player:WaitForChild("PlayerGui")

-- ===== INTERFACE DO SISTEMA DE KEY (TELA DE LOGIN) =====
local keyGui = Instance.new("ScreenGui")
keyGui.Name = "KeySystem_Elite"
keyGui.ResetOnSpawn = false
keyGui.Parent = pgui

local keyFrame = Instance.new("Frame")
keyFrame.Size = UDim2.new(0, 350, 0, 250)
keyFrame.Position = UDim2.new(0.5, -175, 0.5, -125)
keyFrame.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
keyFrame.BorderSizePixel = 0
keyFrame.Active = true
keyFrame.Draggable = true 
keyFrame.Parent = keyGui
Instance.new("UICorner", keyFrame)

local keyTitle = Instance.new("TextLabel")
keyTitle.Size = UDim2.new(1, 0, 0, 50)
keyTitle.Text = "ELITE ADMIN | KEY SYSTEM"
keyTitle.TextColor3 = Color3.fromRGB(255, 170, 0)
keyTitle.Font = Enum.Font.GothamBlack
keyTitle.TextSize = 18
keyTitle.BackgroundTransparency = 1
keyTitle.Parent = keyFrame

local keyInput = Instance.new("TextBox")
keyInput.Size = UDim2.new(0.8, 0, 0, 45)
keyInput.Position = UDim2.new(0.1, 0, 0.25, 0)
keyInput.PlaceholderText = "Cole a Key aqui..."
keyInput.Text = ""
keyInput.Font = Enum.Font.Gotham
keyInput.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
keyInput.TextColor3 = Color3.new(1, 1, 1)
keyInput.TextSize = 14
keyInput.Parent = keyFrame
Instance.new("UICorner", keyInput)

-- BOTÃO DE VERIFICAR KEY
local checkBtn = Instance.new("TextButton")
checkBtn.Size = UDim2.new(0.8, 0, 0, 45)
checkBtn.Position = UDim2.new(0.1, 0, 0.48, 0)
checkBtn.Text = "ATIVAR SCRIPT"
checkBtn.BackgroundColor3 = Color3.fromRGB(0, 180, 0)
checkBtn.TextColor3 = Color3.new(1, 1, 1)
checkBtn.Font = Enum.Font.GothamBold
checkBtn.TextSize = 16
checkBtn.Parent = keyFrame
Instance.new("UICorner", checkBtn)

-- LINHA DIVISORA
local line = Instance.new("Frame")
line.Size = UDim2.new(0.8, 0, 0, 1)
line.Position = UDim2.new(0.1, 0, 0.72, 0)
line.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
line.BorderSizePixel = 0
line.Parent = keyFrame

-- BOTÃO PARA COPIAR LINK (ALTERADO)
local copyBtn = Instance.new("TextButton")
copyBtn.Size = UDim2.new(0.8, 0, 0, 35)
copyBtn.Position = UDim2.new(0.1, 0, 0.8, 0)
copyBtn.Text = "COPIAR LINK" -- Alterado conforme pedido
copyBtn.BackgroundColor3 = Color3.fromRGB(88, 101, 242)
copyBtn.TextColor3 = Color3.new(1, 1, 1)
copyBtn.Font = Enum.Font.GothamBold
copyBtn.TextSize = 12
copyBtn.Parent = keyFrame
Instance.new("UICorner", copyBtn)

-- LÓGICA DE COPIAR LINK
copyBtn.MouseButton1Click:Connect(function()
    if setclipboard then
        setclipboard(DISCORD_LINK)
        copyBtn.Text = "LINK COPIADO!"
        copyBtn.BackgroundColor3 = Color3.fromRGB(255, 170, 0)
        task.wait(2)
        copyBtn.Text = "COPIAR LINK"
        copyBtn.BackgroundColor3 = Color3.fromRGB(88, 101, 242)
    else
        copyBtn.Text = "SEM SUPORTE"
        task.wait(2)
        copyBtn.Text = "COPIAR LINK"
    end
end)

-- =================================================
-- FUNÇÃO DO SEU SCRIPT PRINCIPAL (ELITE ADMIN)
-- =================================================
local function IniciarEliteAdmin()
    local UserInputService = game:GetService("UserInputService")
    local RunService = game:GetService("RunService")
    local TweenService = game:GetService("TweenService")

    local character = player.Character or player.CharacterAdded:Wait()
    local humanoid

    local MAX_SPEED = 1000
    local MAX_JUMP = 1000
    local FLY_SPEED = 120
    local IMAGE_ID = "rbxassetid://COLE_AQUI_O_ID"

    local function setupCharacter(char)
        character = char
        humanoid = character:WaitForChild("Humanoid")
    end

    setupCharacter(character)
    player.CharacterAdded:Connect(setupCharacter)

    local gui = Instance.new("ScreenGui")
    gui.Name = "EliteAdmin"
    gui.ResetOnSpawn = false
    gui.Parent = pgui

    -- AVISO NO CANTO
    local hint = Instance.new("TextLabel")
    hint.Size = UDim2.new(0,300,0,40)
    hint.Position = UDim2.new(0,20,1,-60)
    hint.BackgroundColor3 = Color3.fromRGB(0,0,0)
    hint.BackgroundTransparency = 0.3
    hint.TextColor3 = Color3.new(1,1,1)
    hint.Font = Enum.Font.GothamBold
    hint.TextScaled = true
    hint.Text = "Pressione K para abrir o Painel"
    hint.Parent = gui
    Instance.new("UICorner", hint)

    task.delay(10, function()
        if hint then
            TweenService:Create(hint, TweenInfo.new(1), {TextTransparency = 1, BackgroundTransparency = 1}):Play()
            task.wait(1)
            hint:Destroy()
        end
    end)

    -- PAINEL PRINCIPAL
    local main = Instance.new("Frame")
    main.Size = UDim2.new(0,500,0,400)
    main.Position = UDim2.new(0.5,-250,0.5,-200)
    main.BackgroundColor3 = Color3.fromRGB(15,15,15)
    main.Visible = false
    main.Parent = gui
    Instance.new("UICorner", main)

    local bg = Instance.new("ImageLabel")
    bg.Size = UDim2.new(1,0,1,0)
    bg.BackgroundTransparency = 1
    bg.Image = IMAGE_ID
    bg.ScaleType = Enum.ScaleType.Fit
    bg.ImageTransparency = 0.2
    bg.Parent = main

    local titleBar = Instance.new("TextLabel")
    titleBar.Size = UDim2.new(1,0,0,50)
    titleBar.BackgroundTransparency = 1
    titleBar.Text = "ELITE ADMIN"
    titleBar.TextScaled = true
    titleBar.Font = Enum.Font.GothamBlack
    titleBar.TextColor3 = Color3.fromRGB(255,170,0)
    titleBar.Parent = main

    -- SISTEMA DE ARRASTAR
    local dragging, dragStart, startPos
    titleBar.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 then
            dragging = true
            dragStart = input.Position
            startPos = main.Position
        end
    end)
    titleBar.InputEnded:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 then dragging = false end
    end)
    UserInputService.InputChanged:Connect(function(input)
        if dragging and input.UserInputType == Enum.UserInputType.MouseMovement then
            local delta = input.Position - dragStart
            main.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
        end
    end)

    -- CRIADOR DE BOTÕES
    local function createButton(text, y)
        local btn = Instance.new("TextButton")
        btn.Size = UDim2.new(0.8,0,0,45)
        btn.Position = UDim2.new(0.1,0,0,y)
        btn.Text = text
        btn.BackgroundColor3 = Color3.fromRGB(0,0,0)
        btn.BackgroundTransparency = 0.3
        btn.TextColor3 = Color3.new(1,1,1)
        btn.Font = Enum.Font.GothamBold
        btn.TextScaled = true
        btn.Parent = main
        Instance.new("UICorner", btn)
        return btn
    end

    local speed = 16
    local speedBtn = createButton("Speed: 16", 70)
    speedBtn.MouseButton1Click:Connect(function()
        speed = (speed + 100 > MAX_SPEED) and 16 or speed + 100
        if humanoid then humanoid.WalkSpeed = speed end
        speedBtn.Text = "Speed: "..speed
    end)

    local jump = 50
    local jumpBtn = createButton("Jump: 50", 130)
    jumpBtn.MouseButton1Click:Connect(function()
        jump = (jump + 100 > MAX_JUMP) and 50 or jump + 100
        if humanoid then humanoid.JumpPower = jump end
        jumpBtn.Text = "Jump: "..jump
    end)

    local flying = false
    local bodyVelocity, bodyGyro
    local flyBtn = createButton("Fly: OFF", 190)
    flyBtn.MouseButton1Click:Connect(function()
        flying = not flying
        if flying and character then
            local root = character:WaitForChild("HumanoidRootPart")
            bodyVelocity = Instance.new("BodyVelocity", root)
            bodyVelocity.MaxForce = Vector3.new(1e9,1e9,1e9)
            bodyGyro = Instance.new("BodyGyro", root)
            bodyGyro.MaxTorque = Vector3.new(1e9,1e9,1e9)
            flyBtn.Text = "Fly: ON"
        else
            if bodyVelocity then bodyVelocity:Destroy() end
            if bodyGyro then bodyGyro:Destroy() end
            flyBtn.Text = "Fly: OFF"
        end
    end)

    RunService.RenderStepped:Connect(function()
        if flying and character then
            local root = character:FindFirstChild("HumanoidRootPart")
            if root and bodyVelocity then
                bodyVelocity.Velocity = workspace.CurrentCamera.CFrame.LookVector * FLY_SPEED
                bodyGyro.CFrame = workspace.CurrentCamera.CFrame
            end
        end
    end)

    local noclip = false
    local noclipBtn = createButton("Noclip: OFF", 250)
    noclipBtn.MouseButton1Click:Connect(function()
        noclip = not noclip
        noclipBtn.Text = noclip and "Noclip: ON" or "Noclip: OFF"
    end)

    RunService.Stepped:Connect(function()
        if noclip and character then
            for _, part in pairs(character:GetDescendants()) do
                if part:IsA("BasePart") then part.CanCollide = false end
            end
        end
    end)

    UserInputService.InputBegan:Connect(function(input, processed)
        if not processed and input.KeyCode == Enum.KeyCode.K then
            main.Visible = not main.Visible
        end
    end)
end

-- =================================================
-- LÓGICA DE VERIFICAÇÃO FINAL
-- =================================================
checkBtn.MouseButton1Click:Connect(function()
    if keyInput.Text == VALID_KEY then
        keyGui:Destroy() 
        IniciarEliteAdmin() 
    else
        checkBtn.Text = "KEY INCORRETA!"
        checkBtn.BackgroundColor3 = Color3.fromRGB(150, 0, 0)
        task.wait(1.5)
        checkBtn.Text = "ATIVAR SCRIPT"
        checkBtn.BackgroundColor3 = Color3.fromRGB(0, 180, 0)
    end
end)
