
local fly = false
local speed = 1.5

RegisterCommand("fly", function()
    fly = not fly
    local msg = fly and "✅ تم تفعيل الطيران" or "❌ تم إيقاف الطيران"
    SetNotificationTextEntry("STRING")
    AddTextComponentString(msg)
    DrawNotification(false, false)
end)

CreateThread(function()
    while true do
        Wait(0)
        if fly then
            local ped = PlayerPedId()
            local pos = GetEntityCoords(ped)
            local forward = GetEntityForwardVector(ped)
            if IsControlPressed(0, 32) then pos = pos + forward * speed end
            if IsControlPressed(0, 33) then pos = pos - forward * speed end
            if IsControlPressed(0, 22) then pos = pos + vector3(0,0,speed) end
            if IsControlPressed(0, 36) then pos = pos - vector3(0,0,speed) end
            SetEntityCoordsNoOffset(ped, pos.x, pos.y, pos.z, true, true, true)
            SetEntityVelocity(ped, 0, 0, 0)
            FreezeEntityPosition(ped, true)
        else
            FreezeEntityPosition(PlayerPedId(), false)
        end
    end
end)
