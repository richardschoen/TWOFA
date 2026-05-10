# TWOFA
TWOFA - A 2FA Two-Factor Authentication solution 100% native to the IBM i

+ Create library file - CRTLIB TWOFA
+ Add library TWOFA to library list - ADDLIBLE TWOFA
+ Create source file - CRTSRCPF TWOFA/TWOFA
+ Upload all members to source file TWOFA<br>Change member type same as [readme.txt](https://github.com/vengoal/TWOFA/blob/main/readme.txt).
+ Compile source member INSTALL2FA - <br>CRTCLPGM PGM(TWOFA/INSTALL2FA) SRCFILE(TWOFA/TWOFA) SRCMBR(INSTALL2FA)
+ Create all programs, commands, signon display file, physical file and subsystem TWOFA - Call TWOFA/INSTALL2FA
+ Usage:
  - Start subsystem - STRSBS SBSD(TWOFA/TWOFA)<br> Add this command to your auto startup program to auto start subsystem after IPL.
  - Add 2FA user<br> excute command TWOFASET USER(2fauser) <br> Default only add or replace TOTP key to user, no workstation name limit control.<br> If you want limit user and workstation at th same time, Use SQL to update workstation name after add 2FA user.<br> Reference PF TWOFAPF for field description.
  - Remove 2FA user<br> excute command TWOFASET USER(2fauser) REMOVE(*YES)
  - Default only limit workstation device name TWOFA for demo usage. You can use command ADDWSE to add other workstation (for example TWOFA*) to subsystem TWOFA.<br>And those subsystem workstation name need to be same as Client workstation name on 5250 Emulator setting. <br>
  
  
