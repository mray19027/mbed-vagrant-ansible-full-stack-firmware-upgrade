### Install Vagrant

    https://vagrantup.com

### Install VirtualBox

    https://www.virtualbox.org/

### Clone this repo
    git clone https://github.com/ARMmbed/blah

### Install Ansible >= 2.3.0

    Ubuntu:

      $ sudo apt-get install software-properties-common

      $ sudo apt-add-repository ppa:ansible/ansible

      $ sudo apt-get update

      $ sudo apt-get install ansible

    MacOS:

      brew install ansible


      https://docs.ansible.com/ansible/intro_installation.html

### Create your ARM mbed Cloud credential files

    1) .mbedcloud.json
    2) ~/Downloads/testing/mbedcloud/1.2/production/mbed_cloud_dev_credentials.c
    3) ~/.netrc

    .mbedcloud.json
        {
            "MBED_CLOUD_API_ENDPOINT": "https://api.mbedcloud.com",
            "MBED_CLOUD_API_KEY": "ak_XXXXX",
            "MBED_CLOUD_EMAIL": "my.email@somedomain.com",
            "MBED_CLOUD_PASSWORD": "my_password",
            "MBED_CLOUD_TEAM": "my_team_name"
        }

    mbed_cloud_dev_credentials.c

        https://github.com/ARMmbed/mbed-cloud-client-example/blob/master/README.md#client-credentials

        Copy production/mbed_cloud_dev_credentials.c which you obtained from "https://portal.mbedcloud.com/Developer tools/Certificate"

        ~/Downloads/testing/mbedcloud/1.2/production/mbed_cloud_dev_credentials.c

    .netrc - this is needed to access private repos on GitHub
        machine github.com
        login your_username_here
        password your_password_here OR your_github_api_token_here
    
    ** If you have special characters or spaces in your GitHub password, or
    would prefer to use an API token you can use that in place of the actual
    password.  The preferred method is to use tokens since they can be scoped.
    
    https://github.com/settings/tokens


### Start up the environment
   
    vagrant up

### Connect a FRDM-K64F https://developer.mbed.org/platforms/FRDM-K64F/ board to your USB port

Connect the Ethernet port on the FRDM-K64F to your switch or router.

### Flash the firmware

Copy the firmware binary that was built to the FRDM-K64F board in order
to flash the image.

    cp Source/mbed-os-example-client/BUILD/blah /mnt/MBED

### Connect to the serial port on the FRDM-K64F to reset after flashing and to watch connection details.

    sudo minicom -D /dev/ttyACM0 -b 115200

Type "ctrl + a" let go then "f" to send a break to the board once the
flashing activity led stops blinking rapidly from the copy/flashing step
above.

Now watch for connection messages once the board comes back up

### Test it out!

Point your browser to http://localhost:8080

This is a NodeJS application server running on a virtual machine on
your computer.  It's the one called server if you were to list the VM's
created by typing "vagrant status"

This NodeJS server is running against your mbed Cloud instance interacting
with the REST API.

MyComputer --> ServerVM --> REST API --> mbed Cloud

The data and interaction contained in the NodeJS application is interacting
with the FRDM-K64F.  If everything connected correctly and your credentials
are valid you should be able to see active data and make changes on the
FRDM-K64F from the browser.

*note: since we don't have a publically routable IP address, the NodeJS
       application implements polling to make this demo work everywhere
       if you had a public IP then webhook notifications are the quickest
       and lightest weight choice

FRDM-K4F --> CoAP --> mbed Cloud

### Tear Down

When you're all done tear it down

    vagrant destroy -f
