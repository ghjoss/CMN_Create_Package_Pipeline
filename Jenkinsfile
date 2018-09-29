pipeline
{
    options 
	{
        timeout(time: 5 , unit: 'MINUTES')
    }
    agent 
	{
        node 
		{
            label 'RRMacZOS_CMN'
        }
    }
    environment 
    {
        WS = '/u/CMN/jenkins/pipelines/CrePkgPipe'
    }
    stages
	{
        stage("1")
		{
            steps
			{
                ws("${env.WS}/${env.BUILD_ID}")
				{
                    withEnv(['APPLID="PLAY"', 
							'WREQ="N/A"', 
							'CMNSYS="P"',
 							'REQRNM="Jenkins"',
							'REQRPH="n/a"', 
							'PKTYPE=1', 
							'PKLVL=1', 
							'SITE1=MVS1',
                            'IACNM1="Jenkins"', 
							'IACPH1="n/a"',
							'ICTNM1="Jenkins"', 
							'ICTPH1="n/a"',
                            'INSDT1=20181231', 
							'IFRTM1=1004', 
 							'ITOTM1=1030', 
                            'WAIT_FOR_JOB=180', 
                            'ALVL=PROD', /* TSO DSN Allocate script level (TEST/HOLD/PROD) */
							'DLVL=PROD', /* Archival dataset specifications script level (TEST/HOLD/PROD) */
							'FLVL=PROD', /* Copy / Error processing on batch job completion script level (TEST/HOLD/PROD) */
                            '_BPXK_AUTOCVT=ON', 
							'PIPELINE=Y'])
					{
                        sh 'printenv'
                        //call create package
                        sh returnStdout: true, script: '/bin/sh -ex /usr/lpp/ispw/SUTL/scripts/PROD/ChangeMan.sh CREPKG'
                	    archiveArtifacts allowEmptyArchive: true, artifacts: '*.log,*.rslt,*.tstamp,*.out', caseSensitive: true, defaultExcludes: true, fingerprint: false, onlyIfSuccessful: false 
					}
                }
            }
        }
    }
	post
	{
		always
		{
		    /* I suspect this doesn't work because the post{} processing commands are
		    ** are executing in the @TMP directory. Thus the deleteDir() can't delete
		    ** files because they are active.
		    */
			dir("${env.WS}/${env.BUILD_ID}@TMP")
			{
				deleteDir() /* clean up temp dir */
			}
			echo 'Pipeline complete'
		}
		success
		{
			dir("${env.WS}/${env.BUILD_ID}")
			{
				deleteDir() /* clean up workspace */
			}
			echo 'Successful execution'
		}
		failure
		{
			echo 'Pipeline failure: uss workspace (${env.WS}/${env.BUILD_ID}) datasets have not been deleted'
		}
	}
}
