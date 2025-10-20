-- // GUI SCRIPT - Spectre Hub Player // --
-- // Feito para rodar em LocalScript // --

-- Função utilitária para criar objetos de forma rápida
local function create(class, props)
	local obj = Instance.new(class)
	for i, v in pairs(props) do
		obj[i] = v
	end
	return obj
end

-- // Tela principal //
local player = game.Players.LocalPlayer
local gui = create("ScreenGui", {Parent = player:WaitForChild("PlayerGui"), ResetOnSpawn = false})

-- // Frame principal //
local mainFrame = create("Frame", {
	Parent = gui,
	Size = UDim2.new(0, 200, 0, 250),
	Position = UDim2.new(0.5, -100, 0.5, -125),
	BackgroundColor3 = Color3.fromRGB(30, 30, 30),
	BorderSizePixel = 0,
	Visible = true
})

create("UICorner", {Parent = mainFrame, CornerRadius = UDim.new(0, 8)})

-- // Título //
local title = create("TextLabel", {
	Parent = mainFrame,
	Text = "Spectre Hub Player",
	Size = UDim2.new(1, 0, 0, 30),
	BackgroundColor3 = Color3.fromRGB(45, 45, 45),
	TextColor3 = Color3.fromRGB(255, 255, 255),
	Font = Enum.Font.GothamBold,
	TextSize = 16
})
create("UICorner", {Parent = title, CornerRadius = UDim.new(0, 8)})

-- // Botão para minimizar //
local minimizeButton = create("TextButton", {
	Parent = mainFrame,
	Text = "-",
	Size = UDim2.new(0, 30, 0, 30),
	Position = UDim2.new(1, -35, 0, 0),
	BackgroundColor3 = Color3.fromRGB(70, 70, 70),
	TextColor3 = Color3.fromRGB(255,255,255),
	Font = Enum.Font.GothamBold,
	TextSize = 18
})
create("UICorner", {Parent = minimizeButton, CornerRadius = UDim.new(1, 0)})

-- // Frame dos botões //
local buttonFrame = create("Frame", {
	Parent = mainFrame,
	Size = UDim2.new(1, 0, 1, -35),
	Position = UDim2.new(0, 0, 0, 35),
	BackgroundTransparency = 1
})

-- // Template de botão //
local function makeButton(name, y)
	local btn = create("TextButton", {
		Parent = buttonFrame,
		Text = name,
		Size = UDim2.new(0.8, 0, 0, 35),
		Position = UDim2.new(0.1, 0, 0, y),
		BackgroundColor3 = Color3.fromRGB(60, 60, 60),
		TextColor3 = Color3.fromRGB(255,255,255),
		Font = Enum.Font.Gotham,
		TextSize = 14
	})
	create("UICorner", {Parent = btn, CornerRadius = UDim.new(0, 6)})
	return btn
end

local btnSpeed = makeButton("Speed", 10)
local btnJump = makeButton("Jump", 55)
local btnFly = makeButton("Fly", 100)
local btnNoclip = makeButton("Noclip", 145)

-- // Ícone para reabrir (☰) //
local reopenButton = create("TextButton", {
	Parent = gui,
	Text = "☰",
	Size = UDim2.new(0, 40, 0, 40),
	Position = UDim2.new(0, 20, 0.15, 0), -- 👈 agora mais para cima!
	BackgroundColor3 = Color3.fromRGB(40, 40, 40),
	TextColor3 = Color3.fromRGB(255,255,255),
	Font = Enum.Font.GothamBold,
	TextSize = 20,
	Visible = false
})
create("UICorner", {Parent = reopenButton, CornerRadius = UDim.new(1, 0)})

-- // Funções dos botões //
local char = player.Character or player.CharacterAdded:Wait()
local humanoid = char:WaitForChild("Humanoid")
local flying = false
local noclipping = false

btnSpeed.MouseButton1Click:Connect(function()
	humanoid.WalkSpeed = (humanoid.WalkSpeed == 16) and 50 or 16
end)

btnJump.MouseButton1Click:Connect(function()
	humanoid.JumpPower = (humanoid.JumpPower == 50) and 120 or 50
end)

btnFly.MouseButton1Click:Connect(function()
	flying = not flying
	local HRP = char:WaitForChild("HumanoidRootPart")
	while flying do
		task.wait()
		local moveDir = humanoid.MoveDirection
		HRP.Velocity = Vector3.new(moveDir.X * 50, 0, moveDir.Z * 50)
	end
end)

btnNoclip.MouseButton1Click:Connect(function()
	noclipping = not noclipping
	while noclipping do
		task.wait()
		for _, part in pairs(char:GetDescendants()) do
			if part:IsA("BasePart") then
				part.CanCollide = false
			end
		end
	end
end)

-- // Minimizar e reabrir //
minimizeButton.MouseButton1Click:Connect(function()
	mainFrame.Visible = false
	reopenButton.Visible = true
end)

reopenButton.MouseButton1Click:Connect(function()
	mainFrame.Visible = true
	reopenButton.Visible = false
end)
