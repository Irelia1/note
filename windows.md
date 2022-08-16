https://www.likecs.com/show-204363826.html

### powershell

profile主要用于个性化常用的函数、别名等等。每次加载powershell的时候，都会执行profile中的内容。

查看是否有profile：

```
$profile
```

如果结果是false说明没有。则创建一个。

```powershell
New-Item –Path $Profile –Type File –Force
```

-Force 是强制创建，即使你有了，也创建。

使用记事本编辑你自己的profile：

```
notepad $Profile
```

这个里面，可以输入任何你在ps中输入的命令、函数。

比如，个性化自己的ps界面、常用的自己用的函数等等。

第一个，我们保持控制台提示中显示当前路径不变，但是在输完当前路径后，我们让PowerShell光标换一下行，这样我们既可以很方便地看到当前路径，也可以让光标移到控制台最开始：

```
function prompt
{
  Write-Host("PS: $pwd>")
}
```

另外一种方案是直接把当前路径显示在控制台的标题栏上:

```
function prompt
{
  $host.UI.RawUI.WindowTitle = Get-Location
  'PS> '
}
```

如果你喜欢其中的某个方案，并且想让它在控制台打开时自动执行，可以考虑把它放在$profie配置文件中。

#### get-process|format-list

了解 管道，对象的概念

measure、where、sort、foreach

```powershell
ls | where { $_.Extension -eq ".txt" }
```

```powershell
where { $_.CPU -gt 20}
```

```powershell
Get-Procee | where { $_.ID -gt 4000 } | ForEach { $_.CPU }
```

```text
$students | ForEach { Add-Member -InputObject $_ -MemberType NoteProperty -Name "Sum" -Value ($_.Maths + $_.English)} | Export-Csv sum.csv
```

"ForEach" 有一个别名，百分比符号 %

```text
$students | % { $_.Maths = [int]$_.Maths}
$students | % { $_.English = [int]$_.English}
$students | % { Add-Member -InputObject $_ -MemberType NoteProperty -Name 'Sum' -Value ($_.Maths + $_.English)}
```

首先，我们可以不写"-InputObject"，直接用管道把我们的对象传递给命令即可，命令这就短了不少：

```text
$students | % { $_ | Add-Member -MemberType NoteProperty -Name 'Sum' -Value ($_.Maths + $_.English)}
```

另外，当我们用 Add-Member 的时候，95% 以上的时间里，都在用它添加 NoteProperty。这个命令总是用来添加 NoteProperty，所以设计者加了一个"-NotePropertyMembers" 参数。

```text
$students | % { $_ | Add-Member -NotePropertyMembers @{ 'Sum' = ($_.Maths + $_.English)}
```

这句命令就添加了一个 NoteProperty，其名称为 "Sum"，值则是数学和英语的总和。可以用这个方法添加不止一个 NoteProperty，我们后面学到哈希表的时候会详细说明。

```powershell
[Environment]::SetEnvironmentVariable('JAVA_HOME', 'C:\Program Files\Java\jdk-11.0.13' , 'Machine')
```

```powershell
[Environment]::GetEnvironmentVariable('Path', 'Machine')
```

```powershell
[Environment]::SetEnvironmentVariable('JAVA_HOME', $null, 'Machine')
```

1. 新建、修改、删除系统环境变量需要管理员权限，请使用管理员权限运行 PowerShell；
2. 新建、修改、删除环境变量在系统层面会立即生效，但在该 PowerShell 中并不，需要重启 PowerShell 才能在 PowerShell 中看到改变。

