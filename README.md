local VALID_KEY = "7f9c2a81-b4d3-4e6f-9a2c-5d8e1b7c3f90"
local DISCORD_LINK = "https://discord.gg/nQ3YqHFPCw"

local Players = game:GetService("Players")
local player = Players.LocalPlayer
local pgui = player:WaitForChild("PlayerGui")

-- ===== KEY SYSTEM =====
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
keyTitle.TextColor3 = Color3.fromRGB(255,170,0)
keyTitle.Font = Enum.Font.GothamBlack
keyTitle.TextSize = 18
keyTitle.BackgroundTransparency = 1
keyTitle.Parent = keyFrame

local keyInput = Instance.new("TextBox")
keyInput.Size = UDim2.new(0.8, 0, 0, 45)
keyInput.Position = UDim2.new(0.1, 0, 0.3, 0)
keyInput.PlaceholderText = "Cole a Key aqui..."
keyInput.BackgroundColor3 = Color3.fromRGB(35,35,35)
keyInput.TextColor3 = Color3.new(1,1,1)
keyInput.Font = Enum.Font.Gotham
keyInput.TextSize = 14
keyInput.Parent = keyFrame
Instance.new("UICorner", keyInput)

local checkBtn = Instance.new("TextButton")
checkBtn.Size = UDim2.new(0.8, 0, 0, 45)
checkBtn.Position = UDim2.new(0.1, 0, 0.55, 0)
checkBtn.Text = "ATIVAR SCRIPT"
checkBtn.BackgroundColor3 = Color3.fromRGB(0,180,0)
checkBtn.TextColor3 = Color3.new(1,1,1)
checkBtn.Font = Enum.Font.GothamBold
checkBtn.TextSize = 16
checkBtn.Parent = keyFrame
Instance.new("UICorner", checkBtn)

-- ===== SCRIPT PRINCIPAL =====
local function IniciarEliteAdmin()

    local UserInputService = game:GetService("UserInputService")
    local RunService = game:GetService("RunService")

    local character = player.Character or player.CharacterAdded:Wait()
    local humanoid = character:WaitForChild("Humanoid")

    player.CharacterAdded:Connect(function(char)
        character = char
        humanoid = character:WaitForChild("Humanoid")
    end)

    local gui = Instance.new("ScreenGui")
    gui.Name = "EliteAdmin"
    gui.ResetOnSpawn = false
    gui.Parent = pgui

    local main = Instance.new("Frame")
    main.Size = UDim2.new(0,500,0,400)
    main.Position = UDim2.new(0.5,-250,0.5,-200)
    main.BackgroundColor3 = Color3.fromRGB(15,15,15)
    main.Visible = false
    main.Active = true
    main.Draggable = true
    main.Parent = gui
    Instance.new("UICorner", main)

    local title = Instance.new("TextLabel")
    title.Size = UDim2.new(1,0,0,50)
    title.Text = "ELITE ADMIN"
    title.BackgroundTransparency = 1
    title.TextScaled = true
    title.Font = Enum.Font.GothamBlack
    title.TextColor3 = Color3.fromRGB(255,170,0)
    title.Parent = main

    -- SPEED BOX
    local speedBox = Instance.new("TextBox")
    speedBox.Size = UDim2.new(0.8,0,0,45)
    speedBox.Position = UDim2.new(0.1,0,0,70)
    speedBox.PlaceholderText = "Digite a Speed..."
    speedBox.BackgroundColor3 = Color3.fromRGB(0,0,0)
    speedBox.TextColor3 = Color3.new(1,1,1)
    speedBox.Font = Enum.Font.GothamBold
    speedBox.TextScaled = true
    speedBox.Parent = main
    Instance.new("UICorner", speedBox)

    speedBox.FocusLost:Connect(function(enter)
        if enter then
            local value = tonumber(speedBox.Text)
            if value and humanoid then
                humanoid.WalkSpeed = math.clamp(value,0,1000)
                speedBox.Text = "Speed: "..humanoid.WalkSpeed
            else
                speedBox.Text = "Valor inválido"
            end
        end
    end)

    -- JUMP BOX
    local jumpBox = Instance.new("TextBox")
    jumpBox.Size = UDim2.new(0.8,0,0,45)
    jumpBox.Position = UDim2.new(0.1,0,0,130)
    jumpBox.PlaceholderText = "Digite o Jump..."
    jumpBox.BackgroundColor3 = Color3.fromRGB(0,0,0)
    jumpBox.TextColor3 = Color3.new(1,1,1)
    jumpBox.Font = Enum.Font.GothamBold
    jumpBox.TextScaled = true
    jumpBox.Parent = main
    Instance.new("UICorner", jumpBox)

    jumpBox.FocusLost:Connect(function(enter)
        if enter then
            local value = tonumber(jumpBox.Text)
            if value and humanoid then
                humanoid.JumpPower = math.clamp(value,0,1000)
                jumpBox.Text = "Jump: "..humanoid.JumpPower
            else
                jumpBox.Text = "Valor inválido"
            end
        end
    end)

    -- FLY
    local flying = false
    local bodyVelocity
    local flyBtn = Instance.new("TextButton")
    flyBtn.Size = UDim2.new(0.8,0,0,45)
    flyBtn.Position = UDim2.new(0.1,0,0,190)
    flyBtn.Text = "Fly: OFF"
    flyBtn.BackgroundColor3 = Color3.fromRGB(0,0,0)
    flyBtn.TextColor3 = Color3.new(1,1,1)
    flyBtn.Font = Enum.Font.GothamBold
    flyBtn.TextScaled = true
    flyBtn.Parent = main
    Instance.new("UICorner", flyBtn)

    flyBtn.MouseButton1Click:Connect(function()
        flying = not flying
        if flying then
            local root = character:WaitForChild("HumanoidRootPart")
            bodyVelocity = Instance.new("BodyVelocity", root)
            bodyVelocity.MaxForce = Vector3.new(1e9,1e9,1e9)
            flyBtn.Text = "Fly: ON"
        else
            if bodyVelocity then bodyVelocity:Destroy() end
            flyBtn.Text = "Fly: OFF"
        end
    end)

    RunService.RenderStepped:Connect(function()
        if flying and bodyVelocity then
            bodyVelocity.Velocity = workspace.CurrentCamera.CFrame.LookVector * 120
        end
    end)

    -- NOCLIP
    local noclip = false
    local noclipBtn = Instance.new("TextButton")
    noclipBtn.Size = UDim2.new(0.8,0,0,45)
    noclipBtn.Position = UDim2.new(0.1,0,0,250)
    noclipBtn.Text = "Noclip: OFF"
    noclipBtn.BackgroundColor3 = Color3.fromRGB(0,0,0)
    noclipBtn.TextColor3 = Color3.new(1,1,1)
    noclipBtn.Font = Enum.Font.GothamBold
    noclipBtn.TextScaled = true
    noclipBtn.Parent = main
    Instance.new("UICorner", noclipBtn)

    noclipBtn.MouseButton1Click:Connect(function()
        noclip = not noclip
        noclipBtn.Text = noclip and "Noclip: ON" or "Noclip: OFF"
    end)

    RunService.Stepped:Connect(function()
        if noclip and character then
            for _,v in pairs(character:GetDescendants()) do
                if v:IsA("BasePart") then
                    v.CanCollide = false
                end
            end
        end
    end)

    UserInputService.InputBegan:Connect(function(input,gp)
        if not gp and input.KeyCode == Enum.KeyCode.K then
            main.Visible = not main.Visible
        end
    end)
end

checkBtn.MouseButton1Click:Connect(function()
    if keyInput.Text == VALID_KEY then
        keyGui:Destroy()
        IniciarEliteAdmin()
    else
        checkBtn.Text = "KEY INCORRETA!"
        checkBtn.BackgroundColor3 = Color3.fromRGB(150,0,0)
        task.wait(1.5)
        checkBtn.Text = "ATIVAR SCRIPT"
        checkBtn.BackgroundColor3 = Color3.fromRGB(0,180,0)
    end
end)
