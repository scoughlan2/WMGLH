# WMGLH

## Setup of pipeline on cloud workstation
```
dx select (WMGLH_sandbox)

dx run --instance-type mem1_ssd2_v2_x16 app-cloud_workstation -imax_session_length="48h" --ssh -y

unset DX_WORKSPACE_ID
dx cd $DX_PROJECT_CONTEXT_ID:

dx cd Ruffus_pipeline
dx download MiSeq-Universal-Nextera-pipeline-2.0.2.tar.gz
tar -xzvf MiSeq-Universal-Nextera-pipeline-2.0.2.tar.gz
cd MiSeq-Universal-Nextera-pipeline-2.0.2

dx download -r docker
cd docker 
docker load < wmglh.docker.tar.gz
cd ..

dx download CLL-config-sc.txt
dx download -r missing_files
dx cd ..
dx download -r example_data/180524_M04099_0184_000000000-BMPJT/
mv 180524_M04099_0184_000000000-BMPJT example_data
cd example_data
gunzip SampleSheet*.csv.gz 
cd ..

mkdir OUT

dx download MiSeq-master-pipeline-v2.0.2-sc.py


## Other
#If need to build docker image from dockerfile
cd docker
docker build . -t wmglh/wmglh
docker save wmglh/wmglh | gzip > wmglh.2.docker.tar.gz
rm -f wmglh.docker.tar.gz
mv wmglh.2.docker.tar.gz wmglh.docker.tar.gz
dx cd Ruffus_pipeline/docker
dx upload wmglh.docker.tar.gz
dx cd ../..
cd ..



docker run --rm -v /home/dnanexus/MiSeq-Universal-Nextera-pipeline-2.0.2:/home/worker/MiSeq-Universal-Nextera-pipeline-2.0.2 wmglh/wmglh python3 /home/worker/MiSeq-Universal-Nextera-pipeline-2.0.2/MiSeq-master-pipeline-v2.0.2.py  -c /home/worker/MiSeq-Universal-Nextera-pipeline-2.0.2/CLL-config-sc.txt -s  /home/worker/MiSeq-Universal-Nextera-pipeline-2.0.2/example_data/SampleSheet.csv -fastq /home/worker/MiSeq-Universal-Nextera-pipeline-2.0.2/example_data/FASTQs/ -interop /home/worker/MiSeq-Universal-Nextera-pipeline-2.0.2/example_data/InterOp/ -n /home/worker/workdir -out /home/worker/output4/


may need to add this line to script
logfile = open("%s.commandline_usage_logfile" % (args.named_directory), "x")
```
