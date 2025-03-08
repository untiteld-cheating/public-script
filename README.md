local StarterGui = game:GetService("StarterGui")

local message = "Chat fill"
local numRepeats = 50

-- Initial color (red)
local red = 204
local green = 0
local blue = 0

-- Target color (orange)
local targetRed = 255
local targetGreen = 102
local targetBlue = 0

-- Calculate color change per iteration
local redChange = (targetRed - red) / numRepeats
local greenChange = (targetGreen - green) / numRepeats
local blueChange = (targetBlue - blue) / numRepeats

for i = 1, numRepeats do
  -- Send the message
  StarterGui:SetCore("ChatMakeSystemMessage", {
    Text = message,
    Color = Color3.fromRGB(red, green, blue)
  })

  -- Update the color components
  red = red + redChange
  green = green + greenChange
  blue = blue + blueChange

  -- Add a small delay
  wait(0.1)
end
