#  WSL

## Создания моста (чтобы соединить ip wsl)
https://randombytes.substack.com/p/bridged-networking-under-wsl?s=r
1. [Создание моста](https://learn.microsoft.com/en-us/windows-server/virtualization/hyper-v/get-started/create-a-virtual-switch-for-hyper-v-virtual-machines?tabs=powershell#create-a-virtual-switch)
  virtmgmt.msc - запустить менеджер hyper-v,
  далее в коммутаторе создать внешнюю сеть
```
netsh interface portproxy add v4tov4 listenport=8080 listenaddress=0.0.0.0 connectport=8080 connectaddress=$($(wsl hostname -I).Trim());
```

```
netsh interface portproxy show v4tov4
```

4. Создать файл `.wslconfig (C:\Users\TiroZit\.wslconfig)`
```
[wsl2]
networkingMode=bridged
vmSwitch=bridge
```
```
Get-NetAdapter -Name *
```
```
New-VMSwitch -Name bridge  -NetAdapterName Ethernet
```

```
wsl --shutdown
```

