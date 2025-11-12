-- // HUB RAYLLOW BRAINROT // --
local player = game.Players.LocalPlayer
local mouse = player:GetMouse()

-- GUI
local gui = Instance.new("ScreenGui", player:WaitForChild("PlayerGui"))
local frame = Instance.new("Frame", gui)
frame.Size = UDim2.new(0, 220, 0, 180)
frame.Position = UDim2.new(0.02, 0, 0.3, 0)
frame.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
frame.BorderSizePixel = 0
frame.Active = true
frame.Draggable = true

local uiCorner = Instance.new("UICorner", frame)
uiCorner.CornerRadius = UDim.new(0, 10)

local function newButton(txt, y)
	local b = Instance.new("TextButton", frame)
	b.Size = UDim2.new(1, -20, 0, 40)
	b.Position = UDim2.new(0, 10, 0, y)
	b.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
	b.TextColor3 = Color3.fromRGB(255, 255, 255)
	b.Text = txt
	b.Font = Enum.Font.GothamBold
	b.TextSize = 14
	local corner = Instance.new("UICorner", b)
	corner.CornerRadius = UDim.new(0, 8)
	return b
end

local deleteMode = false
local noclipMode = false

local delBtn = newButton("🧨 Ativar Modo Apagar", 10)
local flyBtn = newButton("☁️ Noclip / Invisível", 60)
local lightBtn = newButton("⚙️ Modo Leve", 110)

-- Modo apagar
delBtn.MouseButton1Click:Connect(function()
	deleteMode = not deleteMode
	delBtn.Text = deleteMode and "❌ Clique em algo pra sumir" or "🧨 Ativar Modo Apagar"
	if deleteMode then
		mouse.Button1Down:Connect(function()
			if deleteMode and mouse.Target then
				mouse.Target:Destroy()
			end
		end)
	end
end)

-- Noclip + Invisível + Voo
flyBtn.MouseButton1Click:Connect(function()
	noclipMode = not noclipMode
	flyBtn.Text = noclipMode and "☁️ Noclip Ativo" or "☁️ Noclip / Invisível"

	local char = player.Character
	if not char then return end

	for _, part in pairs(char:GetDescendants()) do
		if part:IsA("BasePart") then
			part.CanCollide = not noclipMode
			part.Transparency = noclipMode and 0.5 or 0
		end
	end

	if noclipMode then
		local humanoid = char:FindFirstChildOfClass("Humanoid")
		if humanoid then
			local root = char:WaitForChild("HumanoidRootPart")
			while noclipMode do
				task.wait()
				root.Velocity = Vector3.new(0, 0, 0)
				if game:GetService("UserInputService"):IsKeyDown(Enum.KeyCode.Space) then
					root.CFrame += Vector3.new(0, 1, 0)
				elseif game:GetService("UserInputService"):IsKeyDown(Enum.KeyCode.LeftControl) then
					root.CFrame -= Vector3.new(0, 1, 0)
				end
			end
		end
	end
end)

-- Modo leve
lightBtn.MouseButton1Click:Connect(function()
	for _, v in pairs(workspace:GetDescendants()) do
		if v:IsA("Light") or v:IsA("ParticleEmitter") or v:IsA("Trail") or v:IsA("Beam") then
			v.Enabled = false
		end
	end
	game.Lighting.GlobalShadows = false
	game.Lighting.FogEnd = 100000
	game.Lighting.Brightness = 1
end)