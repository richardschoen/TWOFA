# TWOFA
TWOFA - A 2FA Two-Factor Authentication solution 100% native to the IBM i

For IBM i OS V7R2 above.

+ NTP service
  - <pre>
    Configure NTP service:
          CHGNTPA RMTSYS('time server host name or IP')
                  AUTOSTART(*YES)
                  POLLITV(1)
                  MINADJ(1000)
                  MAXADJ(1)
                  ADJTHLD(*MAXADJ) 
                  ACTLOG(*POLL)
    
    End NTP service:
          ENDTCPSVR SERVER(*NTP)
    
    Start NTP service     
          STRTCPSVR SERVER(*NTP)</pre>
+ Create library file - CRTLIB TWOFA
+ Add library TWOFA to library list - ADDLIBLE TWOFA
+ Create source file - CRTSRCPF TWOFA/TWOFA
+ Download all files, remove all files filename prior 6 character "TWOFA." as new filename.<br>
  Upload all members to source file TWOFA<br>
  Change member type same as [readme.txt](https://github.com/vengoal/TWOFA/blob/main/readme.txt).
+ Compile source member INSTALL2FA <br>CRTCLPGM PGM(TWOFA/INSTALL2FA) SRCFILE(TWOFA/TWOFA) SRCMBR(INSTALL2FA)
+ Create all programs, commands, signon display file, physical file and subsystem TWOFA - Call TWOFA/INSTALL2FA
+ Usage:
  - Install Google Authenticator APP on your phone 
  - Start subsystem - STRSBS SBSD(TWOFA/TWOFA)<br> Add this command to your auto startup program to auto start subsystem after IPL.
  - Add 2FA user<br>
    + excute command TWOFASET USER(2fauser)
    + Use SQL select statement to get totp key from file TWOFAPF and set the totp key on Google Authenticator APP.
    + Create user TOTP key QR code for Google Authenticator add user setting.
      - CRTPRTF PRTF(TWOFA/QRCODEPRTF) SRCFILE(TWOFA/TWOFA) SRCMBR(QRCODEPRTF)
      - CRTBNDRPG PGM(TWOFA/QRCODEPINR) SRCFILE(TWOFA/TWOFA) SRCMBR(QRCODEPINR)
      - CRTCLPGM  PGM(TWOFA/QRCODEPINC) SRCFILE(TWOFA/TWOFA) SRCMBR(QRCODEPINC)
      - CAll PGM(TWOFA/QRCODEPINC) Parm( '2fauser' '2fauser_email' 'totpkey' ) <br /><pre>
        - Parameters description:
             Get 2fauser and totpkey value from file TWOFAPF.
             2fauser_email 2fauser email
          2fauser's QR code pdf file locate at '/home/twofauser/2fauser.pdf.
      - Use command [SNDSMTPEMM](https://www.ibm.com/docs/en/i/7.6.0?topic=ssw_ibm_i_76/cl/sndsmtpemm.html) send 2fauser QR code to user, >br />then user use Google Authenticator scan QR code to generate PIN Code for IBM i (AS/400).
        <pre>
          SNDSMTPEMM RCP((mailbox1@domain *PRI))
           SUBJECT(some subject) NOTE(some note)
           ATTACH((/home/twofauser/2fause.pdf *PDF *BIN))
          </pre> 
    + Default only add or replace TOTP key to user, no workstation name limit control.<br> If you want limit user and workstation at th same time, Use SQL to update workstation name after add 2FA user.<br> Reference PF TWOFAPF for field description.
  - Remove 2FA user<br> excute command TWOFASET USER(2fauser) REMOVE(*YES)
  - Default only limit workstation device name TWOFA for demo usage. You can use command ADDWSE to add other workstation (for example TWOFA*) to subsystem TWOFA.<br>And those subsystem workstation name need to be same as Client workstation name on 5250 Emulator setting. <br>

+ Reference:
  - totp.c  -  https://gist.github.com/syzdek/eba233ca33e1b5a45a99

  
  
