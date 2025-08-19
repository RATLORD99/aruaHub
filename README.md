--// Load Kavo UI
local Kavo = loadstring(game:HttpGet("https://raw.githubusercontent.com/xHeptc/Kavo-UI-Library/main/source.lua"))()
local KavoWindow = Kavo.CreateLib("AuraHub - Kavo", "DarkTheme")

-- Kavo tab/section (kept from your design)
local Tab = KavoWindow:NewTab("Events")
local Section = Tab:NewSection("Bean Stalk")

Section:NewLabel("Collect Orbs")
Section:NewToggle("BeanStalk Rewards", "AutoCollectOrbs", function(state)
    if state then
        print("Kavo farming ON")
    else
        print("Kavo farming OFF")
    end
end)

-- ===== KAVO MINIMIZE/RESTORE (button + keybind) =====
local KAVO_TITLE = "AuraHub - Kavo" -- your window title (used for detection fallback)
local hidden = false

local function getUiRoot()
    -- use gethui() if provided by executor, else CoreGui
    local ok, ui = pcall(function()
        if typeof(gethui) == "function" then
            return gethui()
        else
            return game:GetService("CoreGui")
        end
    end)
    if ok and ui then return ui end
    return game:GetService("CoreGui")
end

local function findKavoGui()
    local root = getUiRoot()

    -- Most common Kavo UI names
    local sgui = root:FindFirstChild("Kavo UI Library") or root:FindFirstChild("KavoUI") or root:FindFirstChild("KavoUILibrary")
    if sgui and sgui:IsA("ScreenGui") then
        return sgui
    end

    -- Fallback: search for a ScreenGui containing a TextLabel with our window title
    for _, inst in ipairs(root:GetChildren()) do
        if inst:IsA("ScreenGui") then
            local lbl = inst:FindFirstChildWhichIsA("TextLabel", true)
            if lbl and lbl.Text == KAVO_TITLE then
                return inst
            end
        end
    end
    return nil
end

local function setKavoVisible(visible)
    local gui = findKavoGui()
    if not gui then
        warn("Kavo UI not found (cannot toggle).")
        return
    end
    -- Try ScreenGui.Enabled first
    local ok = pcall(function()
        gui.Enabled = visible
    end)
    if not ok then
        -- Fallback: toggle visibility of child GuiObjects
        for _, child in ipairs(gui:GetChildren()) do
            if child:IsA("GuiObject") then
                child.Visible = visible
            end
        end
    end
end

Section:NewButton("Minimize Kavo", "Hide/Show Kavo window", function()
    hidden = not hidden
    setKavoVisible(not hidden)
    print(hidden and "Kavo UI minimized" or "Kavo UI restored")
end)

-- Optional keybind (RightShift) to toggle Kavo UI
Section:NewKeybind("Toggle Kavo (RightShift)", "Hide/Show Kavo window", Enum.KeyCode.RightShift, function()
    hidden = not hidden
    setKavoVisible(not hidden)
end)
-- ===== END KAVO MINIMIZE =====


--// Load Rayfield UI
local Rayfield = loadstring(game:HttpGet("https://sirius.menu/rayfield"))()
local Window = Rayfield:CreateWindow({
    Name = "AuraHub - Rayfield",
    LoadingTitle = "AuraHub Loader",
    LoadingSubtitle = "by You",
    ConfigurationSaving = {
        Enabled = true,
        FolderName = "AuraHub",
        FileName = "Settings"
    }
})

--// Rayfield Example
local MainTab = Window:CreateTab("Main")
local Toggle = MainTab:CreateToggle({
    Name = "Auto Collect (Rayfield)",
    CurrentValue = false,
    Flag = "AutoFarmToggle",
    Callback = function(Value)
        if Value then
            print("Rayfield farming ON")
        else
            print("Rayfield farming OFF")
        end
    end,
})
