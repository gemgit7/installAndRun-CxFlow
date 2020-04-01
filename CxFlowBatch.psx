<#
    Checkmarx CxFlow batch mode runner. Delegates to the CxFlow jar.
    Checkmarx Professional Services

    Usage: .\CxFlowBatch.ps1 [-cxFlowVersion <version>] [-jdk11] [-installDir <installDir>] -config <configYml> -cxTeam <fullCxTeam> -cxProject <cxProject>    
#>
Param(

    [Parameter(Mandatory = $False)]
    [ValidateNotNullOrEmpty()]
    [String]
    $cxFlowVersion = "1.5.4",

    [switch]
    $jdk11,

    [Parameter(Mandatory = $True)]
    [ValidateNotNullOrEmpty()]
    [String] $installDir,
    
    [Parameter(Mandatory = $True)]
    [ValidateNotNullOrEmpty()]
    [String]
    $config,

    [Parameter(Mandatory = $True)]
    [ValidateNotNullOrEmpty()]
    [String] $CxTeam,

    [Parameter(Mandatory = $True)]
    [ValidateNotNullOrEmpty()]
    [String] $CxProject  
)

function createCommand ($installPath) {

    $cxFlowJar = "cx-flow-${cxFlowVersion}.jar"
	if ($jdk11) {
		$cxFlowJar = "cx-flow-11-${cxFlowVersion}.jar"
	}
	
    # Basic command
    $cmd = "java -jar $installPath\$cxFlowJar"

    # Config path provided?
    $cmd += if ($config) { " --spring.config.location=`"$config`"" } else { "" }

    $cmd += " --project"

    # CX Team
    $cmd += if ($CxTeam) { " --cx-team=`"$CxTeam`"" } else { "" }

    # Cx Project   
    $cmd += if ($CxProject) { " --cx-project=`"$CxProject`"" } else { "" }

    $cmd += " --app=`"$CxProject`""
    
    return $cmd

}
function InstallCxFlow() {

	$cxFlowJar = "cx-flow-${cxFlowVersion}.jar"
	if ($jdk11) {
		$cxFlowJar = "cx-flow-11-${cxFlowVersion}.jar"
	}
	
	if (!$installDir) {
    	$installDir = "$PWD\CxFlow"
	}
    $nop = New-Item -ItemType Directory -Force -Path $installDir
    $cxFlowFile = "$installDir\$cxFlowJar"
    
    # URL for Checkmarx CxFlow
    $releaseUrl = "https://github.com/checkmarx-ts/cx-flow/releases/download/$cxFlowVersion/$cxFlowJar"

    # Download CxFlow
    Write-Host "Downloading Checkmarx CxFlow from $releaseUrl to $cxFlowFile"
    (New-Object System.Net.WebClient).DownloadFile($releaseUrl, "$cxFlowFile")

    return $installDir
}


# ========== Execution Entry ========== #

$installPath = InstallCxFlow
$command = createCommand $installPath
Write-Host "Executing command $command"
Invoke-Expression "$command"
