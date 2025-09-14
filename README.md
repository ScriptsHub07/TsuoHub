local Library = {}
Library.__index = Library

-- Cores e configurações
local colors = {
    background = Color3.fromRGB(40, 40, 40),
    primary = Color3.fromRGB(106, 43, 186),
    secondary = Color3.fromRGB(70, 25, 130),
    text = Color3.fromRGB(255, 255, 255),
    black = Color3.fromRGB(20, 20, 20)
}

local transparency = 0.4

-- Funções utilitárias
local function createElement(className, properties)
    local element = Instance.new(className)
    for property, value in pairs(properties) do
        element[property] = value
    end
    return element
end

-- Inicialização da Library
function Library:Init()
    local screenGui = createElement("ScreenGui", {
        Name = "ScriptHubUI",
        ResetOnSpawn = false
    })
    
    self.mainFrame = createElement("Frame", {
        Name = "MainFrame",
        BackgroundColor3 = colors.background,
        BackgroundTransparency = transparency,
        BorderColor3 = colors.primary,
        BorderSizePixel = 2,
        Position = UDim2.new(0.3, 0, 0.2, 0),
        Size = UDim2.new(0.4, 0, 0.6, 0),
        Parent = screenGui
    })
    
    -- Header com título e botões de controle
    local header = createElement("Frame", {
        Name = "Header",
        BackgroundColor3 = colors.primary,
        BackgroundTransparency = 0.2,
        BorderSizePixel = 0,
        Size = UDim2.new(1, 0, 0.08, 0),
        Parent = self.mainFrame
    })
    
    self.titleLabel = createElement("TextLabel", {
        Name = "TitleLabel",
        BackgroundTransparency = 1,
        Position = UDim2.new(0.02, 0, 0, 0),
        Size = UDim2.new(0.6, 0, 1, 0),
        Font = Enum.Font.GothamBold,
        Text = "Script Hub",
        TextColor3 = colors.text,
        TextSize = 16,
        TextXAlignment = Enum.TextXAlignment.Left,
        Parent = header
    })
    
    -- Botões de controle
    local closeBtn = createElement("TextButton", {
        Name = "CloseButton",
        BackgroundColor3 = Color3.fromRGB(200, 60, 60),
        BackgroundTransparency = 0.3,
        BorderSizePixel = 0,
        Position = UDim2.new(0.85, 0, 0.2, 0),
        Size = UDim2.new(0.05, 0, 0.6, 0),
        Font = Enum.Font.Gotham,
        Text = "X",
        TextColor3 = colors.text,
        TextSize = 14,
        Parent = header
    })
    
    local minimizeBtn = createElement("TextButton", {
        Name = "MinimizeButton",
        BackgroundColor3 = colors.secondary,
        BackgroundTransparency = 0.3,
        BorderSizePixel = 0,
        Position = UDim2.new(0.75, 0, 0.2, 0),
        Size = UDim2.new(0.05, 0, 0.6, 0),
        Font = Enum.Font.Gotham,
        Text = "_",
        TextColor3 = colors.text,
        TextSize = 14,
        Parent = header
    })
    
    local maximizeBtn = createElement("TextButton", {
        Name = "MaximizeButton",
        BackgroundColor3 = colors.secondary,
        BackgroundTransparency = 0.3,
        BorderSizePixel = 0,
        Position = UDim2.new(0.8, 0, 0.2, 0),
        Size = UDim2.new(0.05, 0, 0.6, 0),
        Font = Enum.Font.Gotham,
        Text = "□",
        TextColor3 = colors.text,
        TextSize = 14,
        Parent = header
    })
    
    -- Área de conteúdo
    self.contentFrame = createElement("Frame", {
        Name = "ContentFrame",
        BackgroundTransparency = 1,
        Position = UDim2.new(0, 0, 0.08, 0),
        Size = UDim2.new(1, 0, 0.92, 0),
        Parent = self.mainFrame
    })
    
    -- Sistema de notificações
    self.notificationContainer = createElement("Frame", {
        Name = "NotificationContainer",
        BackgroundTransparency = 1,
        Position = UDim2.new(0.02, 0, 0.02, 0),
        Size = UDim2.new(0.96, 0, 0.2, 0),
        Parent = screenGui
    })
    
    -- Tela de loading
    self.loadingScreen = createElement("Frame", {
        Name = "LoadingScreen",
        BackgroundColor3 = colors.black,
        BackgroundTransparency = 0.2,
        Size = UDim2.new(1, 0, 1, 0),
        Visible = false,
        Parent = screenGui
    })
    
    local loadingSpinner = createElement("Frame", {
        Name = "LoadingSpinner",
        AnchorPoint = Vector2.new(0.5, 0.5),
        BackgroundTransparency = 1,
        Position = UDim2.new(0.5, 0, 0.4, 0),
        Size = UDim2.new(0.1, 0, 0.1, 0),
        Parent = self.loadingScreen
    })
    
    local loadingText = createElement("TextLabel", {
        Name = "LoadingText",
        AnchorPoint = Vector2.new(0.5, 0.5),
        BackgroundTransparency = 1,
        Position = UDim2.new(0.5, 0, 0.55, 0),
        Size = UDim2.new(0.6, 0, 0.1, 0),
        Font = Enum.Font.Gotham,
        Text = "Carregando...",
        TextColor3 = colors.text,
        TextSize = 18,
        Parent = self.loadingScreen
    })
    
    -- Eventos dos botões de controle
    closeBtn.MouseButton1Click:Connect(function()
        screenGui:Destroy()
    end)
    
    local isMinimized = false
    minimizeBtn.MouseButton1Click:Connect(function()
        if isMinimized then
            self.mainFrame.Size = UDim2.new(0.4, 0, 0.6, 0)
            self.contentFrame.Visible = true
        else
            self.mainFrame.Size = UDim2.new(0.4, 0, 0.08, 0)
            self.contentFrame.Visible = false
        end
        isMinimized = not isMinimized
    end)
    
    local isMaximized = false
    maximizeBtn.MouseButton1Click:Connect(function()
        if isMaximized then
            self.mainFrame.Size = UDim2.new(0.4, 0, 0.6, 0)
            self.mainFrame.Position = UDim2.new(0.3, 0, 0.2, 0)
        else
            self.mainFrame.Size = UDim2.new(0.95, 0, 0.9, 0)
            self.mainFrame.Position = UDim2.new(0.025, 0, 0.05, 0)
        end
        isMaximized = not isMaximized
    end)
    
    screenGui.Parent = game:GetService("Players").LocalPlayer:WaitForChild("PlayerGui")
    self.screenGui = screenGui
    
    return self
end

-- Função para criar botão de texto
function Library:CreateTextButton(name, properties)
    local defaultProps = {
        BackgroundColor3 = colors.primary,
        BackgroundTransparency = 0.3,
        BorderColor3 = colors.secondary,
        BorderSizePixel = 1,
        Size = UDim2.new(0.9, 0, 0.08, 0),
        Font = Enum.Font.Gotham,
        Text = name,
        TextColor3 = colors.text,
        TextSize = 14
    }
    
    for prop, value in pairs(properties or {}) do
        defaultProps[prop] = value
    end
    
    local button = createElement("TextButton", defaultProps)
    button.Parent = self.contentFrame
    
    return button
end

-- Função para criar label de texto
function Library:CreateTextLabel(name, properties)
    local defaultProps = {
        BackgroundTransparency = 1,
        Size = UDim2.new(0.9, 0, 0.06, 0),
        Font = Enum.Font.Gotham,
        Text = name,
        TextColor3 = colors.text,
        TextSize = 14,
        TextXAlignment = Enum.TextXAlignment.Left
    }
    
    for prop, value in pairs(properties or {}) do
        defaultProps[prop] = value
    end
    
    local label = createElement("TextLabel", defaultProps)
    label.Parent = self.contentFrame
    
    return label
end

-- Função para criar dropdown
function Library:CreateDropdown(name, options, callback)
    local dropdownFrame = createElement("Frame", {
        BackgroundColor3 = colors.primary,
        BackgroundTransparency = 0.3,
        BorderColor3 = colors.secondary,
        BorderSizePixel = 1,
        Size = UDim2.new(0.9, 0, 0.08, 0),
        Parent = self.contentFrame
    })
    
    local dropdownLabel = createElement("TextLabel", {
        BackgroundTransparency = 1,
        Position = UDim2.new(0.02, 0, 0, 0),
        Size = UDim2.new(0.7, 0, 1, 0),
        Font = Enum.Font.Gotham,
        Text = name,
        TextColor3 = colors.text,
        TextSize = 14,
        TextXAlignment = Enum.TextXAlignment.Left,
        Parent = dropdownFrame
    })
    
    local dropdownButton = createElement("TextButton", {
        BackgroundColor3 = colors.secondary,
        BackgroundTransparency = 0.2,
        BorderSizePixel = 0,
        Position = UDim2.new(0.75, 0, 0.15, 0),
        Size = UDim2.new(0.2, 0, 0.7, 0),
        Font = Enum.Font.Gotham,
        Text = "▼",
        TextColor3 = colors.text,
        TextSize = 14,
        Parent = dropdownFrame
    })
    
    local optionsFrame = createElement("ScrollingFrame", {
        BackgroundColor3 = colors.primary,
        BackgroundTransparency = 0.1,
        BorderColor3 = colors.secondary,
        BorderSizePixel = 1,
        Position = UDim2.new(0, 0, 1, 0),
        Size = UDim2.new(1, 0, 0, 0),
        CanvasSize = UDim2.new(0, 0, 0, 0),
        ScrollBarThickness = 5,
        Visible = false,
        Parent = dropdownFrame
    })
    
    local isOpen = false
    
    dropdownButton.MouseButton1Click:Connect(function()
        if isOpen then
            optionsFrame.Visible = false
            optionsFrame.Size = UDim2.new(1, 0, 0, 0)
            dropdownButton.Text = "▼"
        else
            optionsFrame.Visible = true
            optionsFrame.Size = UDim2.new(1, 0, 3, 0)
            dropdownButton.Text = "▲"
        end
        isOpen = not isOpen
    end)
    
    local function addOption(optionText)
        local optionButton = createElement("TextButton", {
            BackgroundColor3 = colors.background,
            BackgroundTransparency = 0.5,
            BorderSizePixel = 0,
            Size = UDim2.new(0.95, 0, 0, 25),
            Font = Enum.Font.Gotham,
            Text = optionText,
            TextColor3 = colors.text,
            TextSize = 14,
            Parent = optionsFrame
        })
        
        optionButton.MouseButton1Click:Connect(function()
            dropdownLabel.Text = optionText
            optionsFrame.Visible = false
            optionsFrame.Size = UDim2.new(1, 0, 0, 0)
            dropdownButton.Text = "▼"
            isOpen = false
            if callback then
                callback(optionText)
            end
        end)
        
        optionsFrame.CanvasSize = UDim2.new(0, 0, 0, optionsFrame.CanvasSize.Y.Offset + 30)
    end
    
    for _, option in ipairs(options) do
        addOption(option)
    end
    
    return dropdownFrame
end

-- Função para criar input de texto
function Library:CreateTextInput(placeholder, callback)
    local textBox = createElement("TextBox", {
        BackgroundColor3 = colors.background,
        BackgroundTransparency = 0.5,
        BorderColor3 = colors.primary,
        BorderSizePixel = 1,
        Size = UDim2.new(0.9, 0, 0.08, 0),
        Font = Enum.Font.Gotham,
        PlaceholderText = placeholder,
        Text = "",
        TextColor3 = colors.text,
        TextSize = 14,
        ClearTextOnFocus = false,
        Parent = self.contentFrame
    })
    
    textBox.FocusLost:Connect(function(enterPressed)
        if enterPressed and callback then
            callback(textBox.Text)
        end
    end)
    
    return textBox
end

-- Função para criar slider
function Library:CreateSlider(name, min, max, defaultValue, callback)
    local sliderFrame = createElement("Frame", {
        BackgroundTransparency = 1,
        Size = UDim2.new(0.9, 0, 0.1, 0),
        Parent = self.contentFrame
    })
    
    local sliderLabel = createElement("TextLabel", {
        BackgroundTransparency = 1,
        Size = UDim2.new(1, 0, 0.4, 0),
        Font = Enum.Font.Gotham,
        Text = name .. ": " .. defaultValue,
        TextColor3 = colors.text,
        TextSize = 14,
        TextXAlignment = Enum.TextXAlignment.Left,
        Parent = sliderFrame
    })
    
    local track = createElement("Frame", {
        BackgroundColor3 = colors.primary,
        BackgroundTransparency = 0.5,
        BorderSizePixel = 0,
        Position = UDim2.new(0, 0, 0.6, 0),
        Size = UDim2.new(1, 0, 0.2, 0),
        Parent = sliderFrame
    })
    
    local fill = createElement("Frame", {
        BackgroundColor3 = colors.secondary,
        BorderSizePixel = 0,
        Size = UDim2.new((defaultValue - min) / (max - min), 0, 1, 0),
        Parent = track
    })
    
    local button = createElement("TextButton", {
        BackgroundColor3 = colors.text,
        BorderSizePixel = 0,
        Position = UDim2.new((defaultValue - min) / (max - min), -8, -1.5, 0),
        Size = UDim2.new(0, 16, 0, 16),
        Text = "",
        Parent = track
    })
    
    local isSliding = false
    
    local function updateValue(value)
        local normalized = math.clamp((value - min) / (max - min), 0, 1)
        fill.Size = UDim2.new(normalized, 0, 1, 0)
        button.Position = UDim2.new(normalized, -8, -1.5, 0)
        local displayValue = math.floor(value * 100) / 100
        sliderLabel.Text = name .. ": " .. displayValue
        
        if callback then
            callback(displayValue)
        end
    end
    
    button.MouseButton1Down:Connect(function()
        isSliding = true
    end)
    
    game:GetService("UserInputService").InputEnded:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 then
            isSliding = false
        end
    end)
    
    game:GetService("UserInputService").InputChanged:Connect(function(input)
        if isSliding and input.UserInputType == Enum.UserInputType.MouseMovement then
            local mousePos = game:GetService("UserInputService"):GetMouseLocation()
            local relativeX = (mousePos.X - track.AbsolutePosition.X) / track.AbsoluteSize.X
            local value = min + (max - min) * math.clamp(relativeX, 0, 1)
            updateValue(value)
        end
    end)
    
    return sliderFrame
end

-- Função para criar scrolling frame
function Library:CreateScrollingFrame(properties)
    local defaultProps = {
        BackgroundColor3 = colors.background,
        BackgroundTransparency = 0.5,
        BorderColor3 = colors.primary,
        BorderSizePixel = 1,
        Size = UDim2.new(0.9, 0, 0.4, 0),
        CanvasSize = UDim2.new(0, 0, 0, 0),
        ScrollBarThickness = 5
    }
    
    for prop, value in pairs(properties or {}) do
        defaultProps[prop] = value
    end
    
    local scrollingFrame = createElement("ScrollingFrame", defaultProps)
    scrollingFrame.Parent = self.contentFrame
    
    local uiListLayout = createElement("UIListLayout", {
        SortOrder = Enum.SortOrder.LayoutOrder,
        Padding = UDim.new(0, 5),
        Parent = scrollingFrame
    })
    
    uiListLayout:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function()
        scrollingFrame.CanvasSize = UDim2.new(0, 0, 0, uiListLayout.AbsoluteContentSize.Y)
    end)
    
    return scrollingFrame
end

-- Função para mostrar notificação
function Library:Notify(title, message, duration)
    duration = duration or 5
    
    local notification = createElement("Frame", {
        BackgroundColor3 = colors.primary,
        BackgroundTransparency = 0.2,
        BorderColor3 = colors.secondary,
        BorderSizePixel = 1,
        Size = UDim2.new(1, 0, 0, 60),
        Parent = self.notificationContainer
    })
    
    local titleLabel = createElement("TextLabel", {
        BackgroundTransparency = 1,
        Position = UDim2.new(0.02, 0, 0.1, 0),
        Size = UDim2.new(0.96, 0, 0.3, 0),
        Font = Enum.Font.GothamBold,
        Text = title,
        TextColor3 = colors.text,
        TextSize = 14,
        TextXAlignment = Enum.TextXAlignment.Left,
        Parent = notification
    })
    
    local messageLabel = createElement("TextLabel", {
        BackgroundTransparency = 1,
        Position = UDim2.new(0.02, 0, 0.5, 0),
        Size = UDim2.new(0.96, 0, 0.4, 0),
        Font = Enum.Font.Gotham,
        Text = message,
        TextColor3 = colors.text,
        TextSize = 12,
        TextXAlignment = Enum.TextXAlignment.Left,
        TextWrapped = true,
        Parent = notification
    })
    
    task.spawn(function()
        task.wait(duration)
        notification:Destroy()
    end)
    
    return notification
end

-- Função para mostrar tela de loading
function Library:ShowLoadingScreen(message)
    self.loadingScreen.Visible = true
    self.loadingScreen:FindFirstChild("LoadingText").Text = message or "Carregando..."
    
    -- Animação do spinner (simplificada)
    local spinner = self.loadingScreen:FindFirstChild("LoadingSpinner")
    if spinner then
        local rotation = 0
        game:GetService("RunService").RenderStepped:Connect(function()
            if not self.loadingScreen.Visible then return end
            rotation = (rotation + 2) % 360
            spinner.Rotation = rotation
        end)
    end
end

-- Função para esconder tela de loading
function Library:HideLoadingScreen()
    self.loadingScreen.Visible = false
end

-- Função para definir o título
function Library:SetTitle(title)
    self.titleLabel.Text = title
end

return Library
