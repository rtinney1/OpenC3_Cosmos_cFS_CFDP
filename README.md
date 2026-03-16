# CFDP for OpenC3

Created By: Randi Tinney

Date Created: March 13 2026

## Moving directories

Move the gsw/microservices directory to the base of your openc3-cosmos instance. Move the gsw/targets/CFDP directory to your targets directory

Move the fsw/cfdp into your instance of cFS, specifically under the apps directory. Make sure to update targets.cmake, your startup.scr script, and add `CFDP_FILEDOWNLOAD_TLM_MID` to your TO app.

## Files for OpenC3

### Updating plugin.txt

In your plugin.txt, make sure you add the following line before your interface:

`TARGET CFDP CFDP`

Then add `MAP_TARGET CFDP` below where you declare your interface.

Finally, add

```
MICROSERVICE CFDP cfdp-microservice
  CMD python cfdp.py 
```

near the bottom of your plugin.txt to register the microservice.

### Updating compose.yaml

Find where the `openc3-operator` container is being created. In volumns, add the lines

```
./send_files:/send_files
./received_files:/received_files
```

The microservice relies on the /send_files and /received_files volumes to uplink/downlink files to and from the fsw. By sharing volumes with the directory, you can easily add files to the /send_files to uplink and copy files from /received_files for downlink