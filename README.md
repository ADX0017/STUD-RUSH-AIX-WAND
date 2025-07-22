local Players = game:GetService("Players")
local player = Players.LocalPlayer
local enemiesFolders = {
	workspace:WaitForChild("Enemies"):WaitForChild("Raid"),
	workspace:WaitForChild("Enemies"):WaitForChild("Allies"),
	workspace:WaitForChild("Enemies"):WaitForChild("Natural"),
}

local ScreenGui = Instance.new("ScreenGui", player:WaitForChild("PlayerGui"))
local Button = Instance.new("TextButton", ScreenGui)
Button.Size = UDim2.new(0, 200, 0, 50)
Button.Position = UDim2.new(0, 20, 0.5, -25)
Button.Text = "SHOT ALL (WAND)"
Button.BackgroundColor3 = Color3.fromRGB(255, 100, 100)
Button.TextScaled = true

Button.MouseButton1Click:Connect(function()
	local wand = player.Character and player.Character:FindFirstChild("Wand")
	if not wand then return end

	local attackEvent = wand:WaitForChild("RemoteEvents"):WaitForChild("Attack")

	for _, folder in ipairs(enemiesFolders) do
		for _, enemy in pairs(folder:GetChildren()) do
			if enemy:IsA("Model") and enemy:FindFirstChild("HumanoidRootPart") then
				local targetPosition = enemy.HumanoidRootPart.Position
				local args = {
					targetPosition
				}
				attackEvent:FireServer(unpack(args))
			end
		end
	end
end)
