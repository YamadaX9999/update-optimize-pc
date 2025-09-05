netsh interface tcp set global rss=disabled
netsh interface tcp set global autotuninglevel=disabled
netsh interface tcp set global ecncapability=disabled
netsh interface tcp set global timestamps=disabled
netsh interface tcp set global initialrto=300
netsh interface tcp set global rsc=disabled
netsh interface tcp set global nonsackrttresiliency=disabled
netsh interface tcp set global maxsynretransmissions=2
netsh interface tcp set global fastopen=disabled
netsh interface tcp set global fastopenfallback=disabled
netsh interface tcp set global hystart=disabled
netsh interface tcp set global prr=disabled
netsh interface tcp set global dca=disabled
netsh interface tcp set global netdma=disabled

netsh interface tcp set security mpp=disabled profiles=disabled
netsh interface tcp set heuristics wsh=disabled forcews=disabled

netsh interface 6to4 set state disabled
netsh interface isatap set state disabled
netsh interface teredo set state disabled

netsh interface tcp set supplemental template=Internet congestionprovider=Newreno
netsh interface tcp set supplemental template=Compat congestionprovider=Newreno
netsh interface tcp set supplemental template=Internetcustom congestionprovider=Newreno
netsh interface tcp set supplemental template=Datacenter congestionprovider=Newreno
netsh interface tcp set supplemental template=Datacentercustom congestionprovider=Newreno

netsh interface ipv4 set global icmpredirects=disabled
netsh interface ipv4 set global taskoffload=disabled
netsh interface ipv4 set global dhcpmediasense=disabled
netsh interface ipv4 set global mediasenseeventlog=disabled
netsh interface ipv4 set global multicastforwarding=disabled
netsh interface ipv4 set global groupforwardedfragments=disabled
netsh interface ipv4 set global randomizeidentifiers=disabled
netsh interface ipv4 set global addressmaskreply=disabled
netsh interface ipv4 set global flowlabel=disabled
netsh interface ipv4 set global loopbacklargemtu=disabled
netsh interface ipv4 set global sourcebasedecmp=disabled
netsh interface ipv4 set subinterface "อีเทอร์เน็ต" mtu=1492 store=persistent

netsh interface ipv6 set global icmpredirects=disabled
netsh interface ipv6 set global taskoffload=disabled
netsh interface ipv6 set global dhcpmediasense=disabled
netsh interface ipv6 set global mediasenseeventlog=disabled
netsh interface ipv6 set global multicastforwarding=disabled
netsh interface ipv6 set global groupforwardedfragments=disabled
netsh interface ipv6 set global randomizeidentifiers=disabled
netsh interface ipv6 set global addressmaskreply=disabled
netsh interface ipv6 set global flowlabel=disabled
netsh interface ipv6 set global loopbacklargemtu=disabled
netsh interface ipv6 set global sourcebasedecmp=disabled

Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Control" -Name "SvcHostSplitThresholdInKB" -Value 0x02000000 -Type DWord

New-Item -Path "HKLM:\SOFTWARE\Policies\Microsoft\Windows\Psched" -Force
Set-ItemProperty -Path "HKLM:\SOFTWARE\Policies\Microsoft\Windows\Psched" -Name "NonBestEffortLimit" -Type DWord -Value 0
Set-DnsClientServerAddress -InterfaceAlias "อีเทอร์เน็ต" -ServerAddresses ("1.1.1.1", "1.0.0.1")

Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters" -Name "Tcp1323Opts" -Type DWord -Value 0
Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters" -Name "TcpWindowSize" -Type DWord -Value 64240

Get-ChildItem HKLM:\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters\Interfaces | ForEach-Object {
    $interfacePath = $_.PSPath
    Set-ItemProperty -LiteralPath $interfacePath -Name "TcpAckFrequency" -Value 1 -Force -ErrorAction SilentlyContinue
    Set-ItemProperty -LiteralPath $interfacePath -Name "TCPNoDelay" -Value 1 -Force -ErrorAction SilentlyContinue
    Set-ItemProperty -LiteralPath $interfacePath -Name "TcpDelAckTicks" -Value 1 -Force -ErrorAction SilentlyContinue
}

Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Control\PriorityControl" -Name "Win32PrioritySeparation" -Value 42 -Type DWord

$mouseKey = "HKLM:\SYSTEM\CurrentControlSet\Services\Mouclass\Parameters"
if (-not (Test-Path $mouseKey)) {
    New-Item -Path "HKLM:\SYSTEM\CurrentControlSet\Services\Mouclass" -Name "Parameters" | Out-Null
}
Set-ItemProperty -Path $mouseKey -Name "MouseDataQueueSize" -Value 16 -Type DWord

$keyboardKey = "HKLM:\SYSTEM\CurrentControlSet\Services\Kbdclass\Parameters"
if (-not (Test-Path $keyboardKey)) {
    New-Item -Path "HKLM:\SYSTEM\CurrentControlSet\Services\Kbdclass" -Name "Parameters" | Out-Null
}
Set-ItemProperty -Path $keyboardKey -Name "KeyboardDataQueueSize" -Value 16 -Type DWord

$adapterName = "อีเทอร์เน็ต"
Set-NetAdapterAdvancedProperty -Name $adapterName -RegistryKeyword "*EEE" -RegistryValue 0
Set-NetAdapterAdvancedProperty -Name $adapterName -RegistryKeyword "*JumboPacket" -RegistryValue 1514
Set-NetAdapterAdvancedProperty -Name $adapterName -RegistryKeyword "*ModernStandbyWoLMagicPacket" -RegistryValue 0
Set-NetAdapterAdvancedProperty -Name $adapterName -RegistryKeyword "AdvancedEEE" -RegistryValue 0
Set-NetAdapterAdvancedProperty -Name $adapterName -RegistryKeyword "EnableGreenEthernet" -RegistryValue 0
Set-NetAdapterAdvancedProperty -Name $adapterName -RegistryKeyword "GigaLite" -RegistryValue 0
Set-NetAdapterAdvancedProperty -Name $adapterName -RegistryKeyword "PowerSavingMode" -RegistryValue 0
Set-NetAdapterAdvancedProperty -Name $adapterName -RegistryKeyword "S5WakeOnLan" -RegistryValue 0
Set-NetAdapterAdvancedProperty -Name $adapterName -RegistryKeyword "*WakeOnMagicPacket" -RegistryValue 0
Set-NetAdapterAdvancedProperty -Name $adapterName -RegistryKeyword "*WakeOnPattern" -RegistryValue 0
Set-NetAdapterAdvancedProperty -Name $adapterName -RegistryKeyword "WolShutdownLinkSpeed" -RegistryValue 0
Set-NetAdapterAdvancedProperty -Name $adapterName -RegistryKeyword "*FlowControl" -RegistryValue 0
Set-NetAdapterAdvancedProperty -Name $adapterName -RegistryKeyword "*interruptModeration" -RegistryValue 0
Set-NetAdapterAdvancedProperty -Name $adapterName -RegistryKeyword "*IPChecksumOffloadIPv4" -RegistryValue 0
Set-NetAdapterAdvancedProperty -Name $adapterName -RegistryKeyword "*LsoV2IPv4" -RegistryValue 0
Set-NetAdapterAdvancedProperty -Name $adapterName -RegistryKeyword "*LsoV2IPv6" -RegistryValue 0
Set-NetAdapterAdvancedProperty -Name $adapterName -RegistryKeyword "*PMARPOffload" -RegistryValue 0
Set-NetAdapterAdvancedProperty -Name $adapterName -RegistryKeyword "*PMNSOffload" -RegistryValue 0
Set-NetAdapterAdvancedProperty -Name $adapterName -RegistryKeyword "*PriorityVLANTag" -RegistryValue 0
Set-NetAdapterAdvancedProperty -Name $adapterName -RegistryKeyword "*RSS" -RegistryValue 0
Set-NetAdapterAdvancedProperty -Name $adapterName -RegistryKeyword "*SelectiveSuspend" -RegistryValue 0
Set-NetAdapterAdvancedProperty -Name $adapterName -RegistryKeyword "*TCPChecksumOffloadIPv4" -RegistryValue 0
Set-NetAdapterAdvancedProperty -Name $adapterName -RegistryKeyword "*TCPChecksumOffloadIPv6" -RegistryValue 0
Set-NetAdapterAdvancedProperty -Name $adapterName -RegistryKeyword "*UDPChecksumOffloadIPv4" -RegistryValue 0
Set-NetAdapterAdvancedProperty -Name $adapterName -RegistryKeyword "*UDPChecksumOffloadIPv6" -RegistryValue 0

Disable-NetAdapterChecksumOffload -Name "อีเทอร์เน็ต"
Disable-NetAdapterLso -Name "อีเทอร์เน็ต"
Disable-NetAdapterPowerManagement -Name "อีเทอร์เน็ต"
Disable-NetAdapterRss -Name "อีเทอร์เน็ต"
Disable-NetAdapterBinding -Name "*"
Enable-NetAdapterBinding -Name "อีเทอร์เน็ต" -ComponentID ms_tcpip
Set-NetFirewallProfile -Profile Domain,Public,Private -Enabled False

$regPath = "HKLM:\SYSTEM\CurrentControlSet\Control\Class\{4d36e972-e325-11ce-bfc1-08002be10318}\0010"

Set-ItemProperty -Path $regPath -Name "*SpeedDuplex" -Value "0"
Set-ItemProperty -Path $regPath -Name "ForceMode" -Value "0"
Set-ItemProperty -Path $regPath -Name "*FlowControl" -Value "0"
Set-ItemProperty -Path $regPath -Name "*PriorityVLANTag" -Value "0"
Set-ItemProperty -Path $regPath -Name "*InterruptModeration" -Value "0"
Set-ItemProperty -Path $regPath -Name "*ReceiveBuffers" -Value "0"
Set-ItemProperty -Path $regPath -Name "*TransmitBuffers" -Value "0"
Set-ItemProperty -Path $regPath -Name "*JumboPacket" -Value "0"
Set-ItemProperty -Path $regPath -Name "*IPChecksumOffloadIPv4" -Value "0"
Set-ItemProperty -Path $regPath -Name "*TCPChecksumOffloadIPv4" -Value "0"
Set-ItemProperty -Path $regPath -Name "*UDPChecksumOffloadIPv4" -Value "0"
Set-ItemProperty -Path $regPath -Name "S5WakeOnLan" -Value "0"
Set-ItemProperty -Path $regPath -Name "PowerDownPll" -Value "0"
Set-ItemProperty -Path $regPath -Name "DeviceRemovable" -Value "0"
Set-ItemProperty -Path $regPath -Name "HwParaMask" -Value 0
Set-ItemProperty -Path $regPath -Name "HwPciOtherFunDevMask" -Value 0
Set-ItemProperty -Path $regPath -Name "HwOptimize" -Value 0
Set-ItemProperty -Path $regPath -Name "HwMode" -Value 0
Set-ItemProperty -Path $regPath -Name "HwOption" -Value 0
Set-ItemProperty -Path $regPath -Name "HwWolCrcVal" -Value 0
Set-ItemProperty -Path $regPath -Name "LogDisconnectEvent" -Value "0"
Set-ItemProperty -Path $regPath -Name "EnableEDT" -Value "0"
Set-ItemProperty -Path $regPath -Name "RIACP" -Value "0"
Set-ItemProperty -Path $regPath -Name "S5NicKeepOverrideMacAddr" -Value "0"
Set-ItemProperty -Path $regPath -Name "S5NicKeepOverrideMacAddrV2" -Value "0"
Set-ItemProperty -Path $regPath -Name "PPSW" -Value "0"
Set-ItemProperty -Path $regPath -Name "GPPSW" -Value "0"
Set-ItemProperty -Path $regPath -Name "S0MgcPkt" -Value "0"
Set-ItemProperty -Path $regPath -Name "HwFPSM" -Value 0
Set-ItemProperty -Path $regPath -Name "SwIML" -Value 0
Set-ItemProperty -Path $regPath -Name "SwIML100" -Value 0
Set-ItemProperty -Path $regPath -Name "SwIMLV2" -Value 0
Set-ItemProperty -Path $regPath -Name "SwIML100V2" -Value 0
Set-ItemProperty -Path $regPath -Name "ENPWMode" -Value "0"
Set-ItemProperty -Path $regPath -Name "Diskless" -Value "0"
Set-ItemProperty -Path $regPath -Name "DisklessOption" -Value "0"
Set-ItemProperty -Path $regPath -Name "TxOptimizeThreshold" -Value 0
Set-ItemProperty -Path $regPath -Name "RxOptimizeThreshold" -Value 0
Set-ItemProperty -Path $regPath -Name "CFHTime" -Value 0
Set-ItemProperty -Path $regPath -Name "LDWTime" -Value 0
Set-ItemProperty -Path $regPath -Name "RMPT" -Value 0
Set-ItemProperty -Path $regPath -Name "CRSPDThreshold" -Value 0
Set-ItemProperty -Path $regPath -Name "ComboPerfAdjust" -Value "0"
Set-ItemProperty -Path $regPath -Name "CHNLWTime" -Value 0
Set-ItemProperty -Path $regPath -Name "CHNLWCnt" -Value 0
Set-ItemProperty -Path $regPath -Name "L1L0sLT" -Value 0
Set-ItemProperty -Path $regPath -Name "MRRSize" -Value 0
Set-ItemProperty -Path $regPath -Name "TDBSize" -Value 0
Set-ItemProperty -Path $regPath -Name "RDBSize" -Value 0
Set-ItemProperty -Path $regPath -Name "HwOptionV2" -Value 0
Set-ItemProperty -Path $regPath -Name "HwOptionV3" -Value 0
Set-ItemProperty -Path $regPath -Name "HwOptionV4" -Value 0
Set-ItemProperty -Path $regPath -Name "HwOptionV5" -Value 0
Set-ItemProperty -Path $regPath -Name "HwBpMask" -Value 0
Set-ItemProperty -Path $regPath -Name "MonitorModeEnabled" -Value "0"
Set-ItemProperty -Path $regPath -Name "UCEM" -Value 0
Set-ItemProperty -Path $regPath -Name "FactoryMode" -Value "0"
Set-ItemProperty -Path $regPath -Name "SwParaMask" -Value 0
Set-ItemProperty -Path $regPath -Name "PowerSavingMode" -Value "0"
Set-ItemProperty -Path $regPath -Name "DisablePBA" -Value "1"
Set-ItemProperty -Path $regPath -Name "*TCPChecksumOffloadIPv6" -Value "0"
Set-ItemProperty -Path $regPath -Name "*UDPChecksumOffloadIPv6" -Value "0"
Set-ItemProperty -Path $regPath -Name "*RSS" -Value "0"
Set-ItemProperty -Path $regPath -Name "*LsoV2IPv4" -Value "0"
Set-ItemProperty -Path $regPath -Name "*LsoV2IPv6" -Value "0"
Set-ItemProperty -Path $regPath -Name "*WakeOnPattern" -Value "0"
Set-ItemProperty -Path $regPath -Name "*WakeOnMagicPacket" -Value "0"
Set-ItemProperty -Path $regPath -Name "*ModernStandbyWoLMagicPacket" -Value "0"
Set-ItemProperty -Path $regPath -Name "*NumRssQueues" -Value "0"
Set-ItemProperty -Path $regPath -Name "WolShutdownLinkSpeed" -Value "0"
Set-ItemProperty -Path $regPath -Name "*PMARPOffload" -Value "0"
Set-ItemProperty -Path $regPath -Name "*PMNSOffload" -Value "0"
Set-ItemProperty -Path $regPath -Name "*EEE" -Value "0"
Set-ItemProperty -Path $regPath -Name "EnableGreenEthernet" -Value "0"
Set-ItemProperty -Path $regPath -Name "AdvancedEEE" -Value "0"
Set-ItemProperty -Path $regPath -Name "GigaLite" -Value "0"
Set-ItemProperty -Path $regPath -Name "EEEMaxSupportSpeed" -Value "0"
Set-ItemProperty -Path $regPath -Name "*SelectiveSuspend" -Value "0"
Set-ItemProperty -Path $regPath -Name "*SSIdleTimeout" -Value "0"
Set-ItemProperty -Path $regPath -Name "RegVlanid" -Value "0"
Set-ItemProperty -Path $regPath -Name "*IfType" -Value 6
Set-ItemProperty -Path $regPath -Name "*MediaType" -Value 0
Set-ItemProperty -Path $regPath -Name "*PhysicalMediaType" -Value 0
Set-ItemProperty -Path $regPath -Name "BusType" -Value "0"
Set-ItemProperty -Path $regPath -Name "Characteristics" -Value 0
Set-ItemProperty -Path $regPath -Name "IfTypePreStart" -Value 0
Set-ItemProperty -Path $regPath -Name "NetLuidIndex" -Value 0
Set-ItemProperty -Path $regPath -Name "BiosSetting" -Value 0
Set-ItemProperty -Path $regPath -Name "ASPM" -Value 0
Set-ItemProperty -Path $regPath -Name "CLKREQ" -Value 0
Set-ItemProperty -Path $regPath -Name "RtHwCapability" -Value 0
Set-ItemProperty -Path $regPath -Name "LTROBFF" -Value 0
Set-ItemProperty -Path $regPath -Name "HwAutoloadMedia" -Value "0"
Set-ItemProperty -Path $regPath -Name "PnPCapabilities" -Value 0

bcdedit /set useplatformtick yes
bcdedit /set disabledynamictick yes
fsutil behavior set DisableDeleteNotify 0
bcdedit /set tscsyncpolicy Enhanced
bcdedit /set usefirmwarepcisettings false
