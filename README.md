# ps-dispatch_carjacking
Fix null alerts
Snippet: ps-dispatch carjacking "null" vehicle fix [Pending]
Description:
Fixes vehicles showing as "null" and "metallic black on metallic black"

Full Snippet::
In qb-vehiclekeys/client/main.lua navigate to

function AttemptPoliceAlert(type)

In this function add the following above your ps-dispatch export for CarJacking

local vehicle = QBCore.Functions.GetClosestVehicle()


The end product should now look something like this:

function AttemptPoliceAlert(type)
    if not AlertSend then
        local chance = Config.PoliceAlertChance
        if GetClockHours() >= 1 and GetClockHours() <= 6 then
            chance = Config.PoliceNightAlertChance
        end
        if math.random() <= chance then
           --TriggerServerEvent('police:server:policeAlert', Lang:t("info.palert") .. type)
           local vehicle = QBCore.Functions.GetClosestVehicle()
           exports['ps-dispatch']:VehicleTheft(vehicle)
        end
        AlertSend = true
        SetTimeout(Config.AlertCooldown, function()
            AlertSend = false
        end)
    end
end
