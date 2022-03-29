 #######Finalized###################33

 Connect-AzAccount
 
$subscriptions = Get-Content -Path "c:\yashyash\Subscriptions.txt" 
$location="uksouth"

Foreach ($sub in $subscriptions) {

$context=Get-AzSubscription -SubscriptionName $sub.name | Set-AzContext

write-host "Azure context set for the subscription, `n$($context.Name)" -f green

$location="uksouth"
$vms=Get-AzVMUsage -Location $location | select @{label="Name";expression={$_.name.LocalizedValue}},currentvalue,limit
$storages=Get-AzStorageUsage -Location $location | Select-Object -Property Name, CurrentValue, Limit
$networks=Get-AzNetworkUsage -Location $location | select @{label="Name";expression={$_.resourcetype}},currentvalue,limit
$result=$vms+$storages+$networks
$result | Export-CSV C:\yashyash\$sub.csv
}


############resource quota limit for the sql server##################
Connect-AzAccount
 
$subscriptions = Get-Content -Path "c:\yash\Subscriptions.txt" 
$location="uksouth"

Foreach ($sub in $subscriptions) {

$context=Get-AzSubscription -SubscriptionName $sub.name | Set-AzContext

write-host "Azure context set for the subscription, `n$($context.Name)" -f green

$location="uksouth"
$vms=Get-AzVMUsage -Location $location | select @{label="Name";expression={$_.name.LocalizedValue}},currentvalue,limit
$storages=Get-AzStorageUsage -Location $location | Select-Object -Property Name, CurrentValue, Limit
$networks=Get-AzNetworkUsage -Location $location | select @{label="Name";expression={$_.resourcetype}},currentvalue,limit
$sqlquota=az sql list-usages --location uksouth -o json | ConvertFrom-Json
$sqlserver= $sqlquota | Select-Object -Property Name, CurrentValue, Limit
$result=$vms+$storages+$networks+$sqlserver
$result | Export-CSV C:\yash\$sub.csv
}
