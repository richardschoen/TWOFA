# TWOFA
TWOFA - A 2FA Two-Factor Authentication solution 100% native to the IBM i

+ Create library file - CRTLIB TWOFA
+ Add library TWOFA to library list - ADDLIBLE TWOFA
+ Create source file - CRTSRCPF TWOFA/TWOFA
+ Upload all members to source file TWOFA
+ Compile source member INSTALL2FA - CRTCLPGM PGM(TWOFA/INSTALL2FA) SRCFILE(TWOFA/TWOFA) SRCMBR(INSTALL2FA)
+ Call INSTALL2FA
