local VALID_KEY = "7f9c2a81-b4d3-4e6f-9a2c-5d8e1b7c3f90" 
local DISCORD_LINK = "https://discord.gg/nQ3YqHFPCw" 
local Players = game:GetService("Players") 
local player = Players.LocalPlayer 
local pgui = player:WaitForChild("PlayerGui") 

-- ===== INTERFACE DO SISTEMA DE KEY ===== 
local keyGui = Instance.new("ScreenGui") 
keyGui.Name = "KeySystem_Elite" 
keyGui.ResetOnSpawn = false 
keyGui.Parent = pgui 

local keyFrame = Instance.new("Frame") 
keyFrame.Size = UDim2.new(0, 350, 0, 250) 
keyFrame.Position = UDim2.new(0.5, -175, 0.5, -125) 
keyFrame.BackgroundColor3 = Color3.fromRGB(25, 25, 25) 
keyFrame.BorderSizePixel = 0 
keyFrame.Active = true 
keyFrame.Draggable = true 
keyFrame.Parent = keyGui 
Instance.new("UICorner", keyFrame).CornerRadius = UDim.new(0, 10)

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
keyInput.Position = UDim2.new(0.1, 0, 0.3, 0) 
keyInput.PlaceholderText = "Cole a Key aqui..." 
keyInput.Text = "" 
keyInput.Font = Enum.Font.Gotham 
keyInput.BackgroundColor3 = Color3.fromRGB(40, 40, 40) 
keyInput.TextColor3 = Color3.new(1, 1, 1) 
keyInput.TextSize = 14 
keyInput.Parent = keyFrame 
Instance.new("UICorner", keyInput) 

local checkBtn = Instance.new("TextButton") 
checkBtn.Size = UDim2.new(0.8, 0, 0, 45) 
checkBtn.Position = UDim2.new(0.1, 0, 0.55, 0) 
checkBtn.Text = "ATIVAR SCRIPT" 
checkBtn.BackgroundColor3 = Color3.fromRGB(0, 180, 0) 
checkBtn.TextColor3 = Color3.new(1, 1, 1) 
checkBtn.Font = Enum.Font.GothamBold 
checkBtn.TextSize = 16 
checkBtn.Parent = keyFrame 
Instance.new("UICorner", checkBtn) 

local keyCopyBtn = Instance.new("TextButton")
keyCopyBtn.Size = UDim2.new(0.8, 0, 0, 35)
keyCopyBtn.Position = UDim2.new(0.1, 0, 0.8, 0)
keyCopyBtn.Text = "COPY LINK"
keyCopyBtn.BackgroundColor3 = Color3.fromRGB(88, 101, 242)
keyCopyBtn.TextColor3 = Color3.new(1, 1, 1)
keyCopyBtn.Font = Enum.Font.GothamBold
keyCopyBtn.TextSize = 12
keyCopyBtn.Parent = keyFrame
Instance.new("UICorner", keyCopyBtn)

keyCopyBtn.MouseButton1Click:Connect(function()
    if setclipboard then
        setclipboard(DISCORD_LINK)
        keyCopyBtn.Text = "LINK COPIADO!"
        task.wait(2)
        keyCopyBtn.Text = "COPY LINK"
    end
end)

-- ================================================= 
-- FUNÇÃO DO SCRIPT PRINCIPAL (ELITE ADMIN) 
-- ================================================= 
local function IniciarEliteAdmin() 
    local UserInputService = game:GetService("UserInputService") 
    local RunService = game:GetService("RunService") 
    
    local walkSpeed = 16
    local jumpPower = 50
    local flySpeed = 1 -- Valor padrão agora é 1
    local flying = false
    local godMode = false
    local noclip = false
    local bV, bG

    local gui = Instance.new("ScreenGui") 
    gui.Name = "EliteAdminMain" 
    gui.ResetOnSpawn = false 
    gui.Parent = pgui 

    local infoLabel = Instance.new("TextLabel")
    infoLabel.Size = UDim2.new(0, 250, 0, 40)
    infoLabel.Position = UDim2.new(0, 10, 1, -50)
    infoLabel.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
    infoLabel.BackgroundTransparency = 0.5
    infoLabel.TextColor3 = Color3.fromRGB(255, 170, 0)
    infoLabel.Text = "Pressione 'K' para abrir o Painel"
    infoLabel.Font = Enum.Font.GothamBold
    infoLabel.TextSize = 14
    infoLabel.Parent = gui
    Instance.new("UICorner", infoLabel)

    local main = Instance.new("Frame") 
    main.Size = UDim2.new(0, 450, 0, 450) 
    main.Position = UDim2.new(0.5, -225, 0.5, -225) 
    main.BackgroundColor3 = Color3.fromRGB(15, 15, 15) 
    main.Visible = false 
    main.Active = true
    main.Draggable = true
    main.Parent = gui 
    Instance.new("UICorner", main).CornerRadius = UDim.new(0, 12)

    local titleBar = Instance.new("TextLabel") 
    titleBar.Size = UDim2.new(1, 0, 0, 60) 
    titleBar.BackgroundTransparency = 1 
    titleBar.Text = "ELITE ADMIN | V3" 
    titleBar.Font = Enum.Font.GothamBlack 
    titleBar.TextColor3 = Color3.fromRGB(255, 170, 0) 
    titleBar.TextSize = 30
    titleBar.Parent = main 

    local function createInput(placeholder, y)
        local box = Instance.new("TextBox")
        box.Size = UDim2.new(0.85, 0, 0, 45)
        box.Position = UDim2.new(0.075, 0, 0, y)
        box.PlaceholderText = placeholder
        box.Text = ""
        box.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
        box.TextColor3 = Color3.new(1, 1, 1)
        box.Font = Enum.Font.GothamSemibold
        box.TextSize = 14
        box.Parent = main
        Instance.new("UICorner", box).CornerRadius = UDim.new(0, 6)
        return box
    end

    local function createButton(text, y)
        local btn = Instance.new("TextButton")
        btn.Size = UDim2.new(0.85, 0, 0, 45)
        btn.Position = UDim2.new(0.075, 0, 0, y)
        btn.Text = text
        btn.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
        btn.TextColor3 = Color3.new(1, 1, 1)
        btn.Font = Enum.Font.GothamBold
        btn.TextSize = 15
        btn.Parent = main
        Instance.new("UICorner", btn).CornerRadius = UDim.new(0, 6)
        return btn
    end

    local speedInput = createInput("Velocidade de Andar", 75)
    speedInput.FocusLost:Connect(function() walkSpeed = tonumber(speedInput.Text) or walkSpeed end)

    local jumpInput = createInput("Força do Pulo", 130)
    jumpInput.FocusLost:Connect(function() jumpPower = tonumber(jumpInput.Text) or jumpPower end)

    local flySpeedInput = createInput("Velocidade do Fly", 185)
    flySpeedInput.FocusLost:Connect(function() 
        local val = tonumber(flySpeedInput.Text)
        if val then flySpeed = val end 
    end)

    local flyBtn = createButton("Fly: OFF", 240)
    flyBtn.MouseButton1Click:Connect(function()
        flying = not flying
        flyBtn.Text = flying and "Fly: ON" or "Fly: OFF"
        flyBtn.TextColor3 = flying and Color3.fromRGB(0, 255, 100) or Color3.new(1,1,1)
        
        local char = player.Character
        if flying and char then
            local root = char:WaitForChild("HumanoidRootPart")
            bV = Instance.new("BodyVelocity", root)
            bV.MaxForce = Vector3.new(1e9, 1e9, 1e9)
            bV.Velocity = Vector3.new(0, 0, 0)
            bG = Instance.new("BodyGyro", root)
            bG.MaxTorque = Vector3.new(1e9, 1e9, 1e9)
            bG.P = 3000
            bG.D = 500
            bG.CFrame = root.CFrame
        else
            if bV then bV:Destroy() end
            if bG then bG:Destroy() end
            if char and char:FindFirstChild("Humanoid") then char.Humanoid.PlatformStand = false end
        end
    end)

    local godBtn = createButton("God Mode: OFF", 295)
    godBtn.MouseButton1Click:Connect(function()
        godMode = not godMode
        godBtn.Text = godMode and "God Mode: ON" or "God Mode: OFF"
    end)

    local noclipBtn = createButton("Noclip: OFF", 350)
    noclipBtn.MouseButton1Click:Connect(function()
        noclip = not noclip
        noclipBtn.Text = noclip and "Noclip: ON" or "Noclip: OFF"
    end)

    -- LÓGICA DE MOVIMENTAÇÃO COM VELOCIDADE NORMALIZADA
    RunService.RenderStepped:Connect(function()
        local char = player.Character
        if char and char:FindFirstChild("Humanoid") and char:FindFirstChild("HumanoidRootPart") then
            local hum = char.Humanoid
            local root = char.HumanoidRootPart
            
            hum.WalkSpeed = walkSpeed
            hum.JumpPower = jumpPower
            if godMode then hum.Health = hum.MaxHealth end

            if flying and bV and bG then
                hum.PlatformStand = true
                local cam = workspace.CurrentCamera
                
                local moveDirection = Vector3.new(0,0,0)
                if UserInputService:IsKeyDown(Enum.KeyCode.W) then moveDirection += cam.CFrame.LookVector end
                if UserInputService:IsKeyDown(Enum.KeyCode.S) then moveDirection -= cam.CFrame.LookVector end
                if UserInputService:IsKeyDown(Enum.KeyCode.A) then moveDirection -= cam.CFrame.RightVector end
                if UserInputService:IsKeyDown(Enum.KeyCode.D) then moveDirection += cam.CFrame.RightVector end
                
                if moveDirection.Magnitude > 0 then
                    -- Multiplicador fixo de 50 para que "1" no painel seja uma velocidade padrão agradável
                    bV.Velocity = moveDirection.Unit * (flySpeed * 50)
                else
                    bV.Velocity = Vector3.new(0, 0, 0)
                end
                bG.CFrame = cam.CFrame
            end

            if noclip then
                for _, part in pairs(char:GetDescendants()) do
                    if part:IsA("BasePart") then part.CanCollide = false end
                end
            end
        end
    end)

    UserInputService.InputBegan:Connect(function(input, processed)
        if not processed and input.KeyCode == Enum.KeyCode.K then
            main.Visible = not main.Visible
        end
    end)
end

checkBtn.MouseButton1Click:Connect(function()
    if keyInput.Text == VALID_KEY then keyGui:Destroy() IniciarEliteAdmin() else checkBtn.Text = "KEY INVÁLIDA" task.wait(1) checkBtn.Text = "ATIVAR SCRIPT" end
end)
