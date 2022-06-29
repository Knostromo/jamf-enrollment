# Apple jamf enrollment

## Background
- If you used/have been provisioned an Apple silicon based work machine, chances are that it would be managed with jamf.
- I guess the reason you might be reading would be:
  - you received the machine but it wasn't setup properly (you can't see any of your org's apps/certs installed e.g. missing `self service` etc).
  - you have been advised/tried resetting the machine (in hoping that it'd trigger the remote management setup process during initial setup but fails everytime).
  - your IT support told you to run `sudo profiles renew -type enrollment` but it doesn't do anything.
  - after 2 hours and 5 transfers on the phone, your IT support left you to your own devices

## Workaround

- So there is a way round this by manully enrolling on jamf and you *don't need to reset the machine*.
- what you need to do is to ask your mate (who has a working machine) to check the jamf server name on their machine:
  - you could easily find these on existing certs
  - e.g. check `MDM Profile` in `System Preferences > Profiles`
    - it tells you about the jamfcloud server address <- **This is what you'd need**
    - it will be in the following form:
      - https://xxxxx.jamfcloud.com/mdm/xxx <- *This will be the jamfcloud MDM url*

- then you can get your machine to enroll manually by based on the jamfcloud MDM url
  - change the MDM url into the following:
    - https://xxxxx.jamfcloud.com/enroll/ <- **Note you only need the domain name and add /enroll/ at the end**
    - probably best to use Safari at these stages
  - it should then ask you for your corporate username/password
  - after passing through security, it will prompt you to install 2 certificates
    - Certificate Authority (CA) & Mobile Device Management (MDM)
      - Note the website may only let you download these certicates one at a time.
      - You'd need to install these certicate manually, to do that you need to open the downloaded certs.
      - then you'd need to go to `System Preferences > Profiles`, you should be able to see the cert you have just downloaded
      - click on `install` on the top right corner
        - e.g. for CA 
        - ![image](https://user-images.githubusercontent.com/47578869/176047006-ec0e7194-c9c0-453b-a61c-150cff96e2c7.png)
        - and for MDM
        - ![image](https://user-images.githubusercontent.com/47578869/176047261-82afbb5e-db0f-49df-b24e-90d70f5d5312.png)
    - please ensure the certs are installed (no warnings on the icon)

- to manually trigger the device enrollment process;
  - open `Terminal`
  - run these commands (with your local root acct password)
    - `sudo profiles renew -type enrollment`
    - `sudo jamf policy`
  - you should see start the process of enrolling your machine/installing all corporate apps
 - enjoy your newly locked down machine :-)

## FAQs
- the command `sudo jamf policy` doesn't do anything
  - try `sudo jamf policy -event trigger` -> ask your local mac support if they have custom trigger for enrolment
 
