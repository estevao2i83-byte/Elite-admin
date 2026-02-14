local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")
local TweenService = game:GetService("TweenService")

local player = Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoid

local MAX_SPEED = 1000
local MAX_JUMP = 1000
local FLY_SPEED = 120

-- 🔥 COLOQUE AQUI O ID DA SUA IMAGEM UPADA COMO DECAL
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
gui.Parent = player:WaitForChild("PlayerGui")

-- ===== AVISO NO CANTO =====
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
	TweenService:Create(hint, TweenInfo.new(1), {TextTransparency = 1, BackgroundTransparency = 1}):Play()
	task.wait(1)
	hint:Destroy()
end)

-- ===== PAINEL =====
local main = Instance.new("Frame")
main.Size = UDim2.new(0,500,0,400)
main.Position = UDim2.new(0.5,-250,0.5,-200)
main.BackgroundColor3 = Color3.fromRGB(15,15,15)
main.Visible = false
main.Parent = gui
Instance.new("UICorner", main)

-- FUNDO IMAGEM
local bg = Instance.new("ImageLabel")
bg.Size = UDim2.new(1,0,1,0)
bg.BackgroundTransparency = 1
bg.Image = IMAGE_ID
bg.ScaleType = Enum.ScaleType.Fit
bg.ImageTransparency = 0.2
bg.Parent = main

-- TÍTULO (área para arrastar)
local titleBar = Instance.new("TextLabel")
titleBar.Size = UDim2.new(1,0,0,50)
titleBar.BackgroundTransparency = 1
titleBar.Text = "ELITE ADMIN"
titleBar.TextScaled = true
titleBar.Font = Enum.Font.GothamBlack
titleBar.TextColor3 = Color3.fromRGB(255,170,0)
titleBar.Parent = main

-- ===== SISTEMA DE ARRASTAR =====
local dragging = false
local dragStart
local startPos

titleBar.InputBegan:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1 then
		dragging = true
		dragStart = input.Position
		startPos = main.Position
	end
end)

titleBar.InputEnded:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1 then
		dragging = false
	end
end)

UserInputService.InputChanged:Connect(function(input)
	if dragging and input.UserInputType == Enum.UserInputType.MouseMovement then
		local delta = input.Position - dragStart
		main.Position = UDim2.new(
			startPos.X.Scale,
			startPos.X.Offset + delta.X,
			startPos.Y.Scale,
			startPos.Y.Offset + delta.Y
		)
	end
end)

-- ===== BOTÕES =====
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

-- SPEED
local speed = 16
local speedBtn = createButton("Speed: 16", 70)

speedBtn.MouseButton1Click:Connect(function()
	speed += 100
	if speed > MAX_SPEED then speed = 16 end
	if humanoid then humanoid.WalkSpeed = speed end
	speedBtn.Text = "Speed: "..speed
end)

-- JUMP
local jump = 50
local jumpBtn = createButton("Jump: 50", 130)

jumpBtn.MouseButton1Click:Connect(function()
	jump += 100
	if jump > MAX_JUMP then jump = 50 end
	if humanoid then humanoid.JumpPower = jump end
	jumpBtn.Text = "Jump: "..jump
end)

-- FLY
local flying = false
local bodyVelocity
local bodyGyro

local flyBtn = createButton("Fly: OFF", 190)

flyBtn.MouseButton1Click:Connect(function()
	flying = not flying
	if flying and character then
		local root = character:WaitForChild("HumanoidRootPart")
		bodyVelocity = Instance.new("BodyVelocity")
		bodyVelocity.MaxForce = Vector3.new(1e9,1e9,1e9)
		bodyVelocity.Parent = root

		bodyGyro = Instance.new("BodyGyro")
		bodyGyro.MaxTorque = Vector3.new(1e9,1e9,1e9)
		bodyGyro.Parent = root

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

-- NOCLIP
local noclip = false
local noclipBtn = createButton("Noclip: OFF", 250)

noclipBtn.MouseButton1Click:Connect(function()
	noclip = not noclip
	noclipBtn.Text = noclip and "Noclip: ON" or "Noclip: OFF"
end)

RunService.Stepped:Connect(function()
	if noclip and character then
		for _, part in pairs(character:GetDescendants()) do
			if part:IsA("BasePart") then
				part.CanCollide = false
			end
		end
	end
end)

-- ABRIR / FECHAR COM K
UserInputService.InputBegan:Connect(function(input, processed)
	if processed then return end
	if input.KeyCode == Enum.KeyCode.K then
		main.Visible = not main.Visible
	end
end)
