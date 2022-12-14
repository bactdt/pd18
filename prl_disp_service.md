# patch prl_disp_app

## 2. patch Signature::SignCheckerImpl

### 2.1 find vtable

#### x86_64

```
__const:00000001009B2A70                         ; `vtable for'Signature::SignCheckerImpl
__const:00000001009B2A70 00 00 00 00 00 00 00 00 _ZTVN9Signature15SignCheckerImplE dq 0  ; DATA XREF: sub_100349A00+28↑o
__const:00000001009B2A70                                                                 ; offset to this
__const:00000001009B2A78 A8 2A 9B 00 01 00 00 00                 dq offset _ZTIN9Signature15SignCheckerImplE ; `typeinfo for'Signature::SignCheckerImpl
__const:00000001009B2A80 00 0B 5B 00 01 00 00 00                 dq offset sub_1005B0B00
__const:00000001009B2A88 10 0B 5B 00 01 00 00 00                 dq offset sub_1005B0B10
__const:00000001009B2A90 80 07 5B 00 01 00 00 00                 dq offset sub_1005B0780
```

#### arm64

```
__const:0000000100988520                         ; `vtable for'Signature::SignCheckerImpl
__const:0000000100988520 00 00 00 00 00 00 00 00 _ZTVN9Signature15SignCheckerImplE DCQ 0 ; DATA XREF: sub_100369410+28↑o
__const:0000000100988520                                                                 ; offset to this
__const:0000000100988528 58 85 98 00 01 00 00 00                 DCQ _ZTIN9Signature15SignCheckerImplE ; `typeinfo for'Signature::SignCheckerImpl
__const:0000000100988530 28 E9 5D 00 01 00 00 00                 DCQ nullsub_201
__const:0000000100988538 2C E9 5D 00 01 00 00 00                 DCQ j___ZdlPv_267
__const:0000000100988540 84 E5 5D 00 01 00 00 00                 DCQ sub_1005DE584
```

### 2.2 patch function `sub_1005B0780`

#### x86_64

```
__text:00000001005B0780 55                                      push    rbp
__text:00000001005B0781 48 89 E5                                mov     rbp, rsp
__text:00000001005B0784 41 57                                   push    r15
__text:00000001005B0786 41 56                                   push    r14
__text:00000001005B0788 41 54                                   push    r12
__text:00000001005B078A 53                                      push    rbx
__text:00000001005B078B 48 81 EC A0 00 00 00                    sub     rsp, 0A0h
__text:00000001005B0792 49 89 CE                                mov     r14, rcx
__text:00000001005B0795 49 89 D7                                mov     r15, rdx
__text:00000001005B0798 49 89 F4                                mov     r12, rsi
__text:00000001005B079B BF D0 0A 00 00                          mov     edi, 0AD0h      ; unsigned __int64
__text:00000001005B07A0 E8 D7 4E 23 00                          call    __Znwm          ; operator new(ulong)
__text:00000001005B07A5 48 89 C3                                mov     rbx, rax
__text:00000001005B07A8 48 89 45 A0                             mov     [rbp+var_60], rax
__text:00000001005B07AC 0F 28 05 DD 8F 38 00                    movaps  xmm0, cs:xmmword_100939790
__text:00000001005B07B3 0F 29 45 90                             movaps  [rbp+var_70], xmm0
__text:00000001005B07B7 48 8D 35 58 8F 31 00                    lea     rsi, aBeginCertifica ; "-----BEGIN CERTIFICATE-----\nMIIHzTCCBb"...
__text:00000001005B07BE BA CC 0A 00 00                          mov     edx, 0ACCh      ; __n
__text:00000001005B07C3 48 89 C7                                mov     rdi, rax        ; __dst
__text:00000001005B07C6 E8 3D 53 23 00                          call    _memcpy
__text:00000001005B07CB C6 83 CC 0A 00 00 00                    mov     byte ptr [rbx+0ACCh], 0
__text:00000001005B07D2 48 8D BD 48 FF FF FF                    lea     rdi, [rbp+var_B8]
__text:00000001005B07D9 48 8D 75 90                             lea     rsi, [rbp+var_70]
__text:00000001005B07DD E8 CE 07 00 00                          call    sub_1005B0FB0
__text:00000001005B07E2 F6 45 90 01                             test    byte ptr [rbp+var_70], 1
__text:00000001005B07E6 74 09                                   jz      short loc_1005B07F1
__text:00000001005B07E8 48 8B 7D A0                             mov     rdi, [rbp+var_60] ; void *
__text:00000001005B07EC E8 61 4E 23 00                          call    __ZdlPv         ; operator delete(void *)
__text:00000001005B07F1
```
opcode

```
55 48 89 E5 41 57 41 56 41 54 53 48 81 EC A0 00
00 00 49 89 CE 49 89 D7 49 89 F4 BF D0 0A 00 00
E8 D7 4E 23 00 48 89 C3 48 89 45 A0 0F 28 05 DD
8F 38 00 0F 29 45 90 48 8D 35 58 8F 31 00 BA CC

```

patch

```
6A 01 58 C3
```

after

```
__text:00000001005B0780                         sub_1005B0780   proc near               ; DATA XREF: __const:00000001009B2A90↓o
__text:00000001005B0780 6A 01                                   push    1
__text:00000001005B0782 58                                      pop     rax
__text:00000001005B0783 C3                                      retn
__text:00000001005B0783                         sub_1005B0780   endp
```


#### arm64

```
__text:00000001005DE584 FF 03 03 D1                             SUB             SP, SP, #0xC0
__text:00000001005DE588 F6 57 09 A9                             STP             X22, X21, [SP,#0xB0+var_20]
__text:00000001005DE58C F4 4F 0A A9                             STP             X20, X19, [SP,#0xB0+var_10]
__text:00000001005DE590 FD 7B 0B A9                             STP             X29, X30, [SP,#0xB0+var_s0]
__text:00000001005DE594 FD C3 02 91                             ADD             X29, SP, #0xB0
__text:00000001005DE598 F3 03 03 AA                             MOV             X19, X3
__text:00000001005DE59C F4 03 02 AA                             MOV             X20, X2
__text:00000001005DE5A0 F5 03 01 AA                             MOV             X21, X1
__text:00000001005DE5A4 00 5A 81 52                             MOV             W0, #0xAD0 ; unsigned __int64
__text:00000001005DE5A8 70 C8 07 94                             BL              __Znwm  ; operator new(ulong)
__text:00000001005DE5AC F6 03 00 AA                             MOV             X22, X0
__text:00000001005DE5B0 E0 2B 00 F9                             STR             X0, [SP,#0xB0+var_60]
__text:00000001005DE5B4 E8 10 00 B0                             ADRP            X8, #xmmword_1007FB2D0@PAGE
__text:00000001005DE5B8 00 B5 C0 3D                             LDR             Q0, [X8,#xmmword_1007FB2D0@PAGEOFF]
__text:00000001005DE5BC E0 83 85 3C                             STUR            Q0, [SP,#0xB0+var_58]
__text:00000001005DE5C0 C1 18 00 F0 21 84 25 91                 ADRL            X1, aBeginCertifica ; "-----BEGIN CERTIFICATE-----\nMIIHzTCCBb"...
__text:00000001005DE5C8 82 59 81 52                             MOV             W2, #0xACC ; __n
__text:00000001005DE5CC A7 CA 07 94                             BL              _memcpy
__text:00000001005DE5D0 DF 32 2B 39                             STRB            WZR, [X22,#0xACC]
__text:00000001005DE5D4 A0 23 01 D1                             SUB             X0, X29, #-var_48
__text:00000001005DE5D8 E1 43 01 91                             ADD             X1, SP, #0xB0+var_60
__text:00000001005DE5DC 00 02 00 94                             BL              j___ZNSt3__112basic_stringIcNS_11char_traitsIcEENS_9allocatorIcEEEC1ERKS5_ ; std::string::basic_string(std::string const&)
__text:00000001005DE5E0 E8 9F C1 39                             LDRSB           W8, [SP,#0xB0+var_58+0xF]
__text:00000001005DE5E4 68 00 F8 36                             TBZ             W8, #0x1F, loc_1005DE5F0
__text:00000001005DE5E8 E0 2B 40 F9                             LDR             X0, [SP,#0xB0+var_60] ; void *
__text:00000001005DE5EC 4A C8 07 94                             BL              __ZdlPv ; operator delete(void *)
__text:00000001005DE5F0
```

opcode

```
FF 03 03 D1 F6 57 09 A9 F4 4F 0A A9 FD 7B 0B A9
FD C3 02 91 F3 03 03 AA F4 03 02 AA F5 03 01 AA
00 5A 81 52 70 C8 07 94 F6 03 00 AA E0 2B 00 F9
E8 10 00 B0 00 B5 C0 3D E0 83 85 3C C1 18 00 F0
```

patch

```
20 00 80 D2 C0 03 5F D6
```

after

```
__text:00000001005DE584                         sub_1005DE584                           ; DATA XREF: __const:0000000100988540↓o
__text:00000001005DE584 20 00 80 D2                             MOV             X0, #1
__text:00000001005DE588 C0 03 5F D6                             RET
__text:00000001005DE588                         ; End of function sub_1005DE584
```

## 2. patch /usr/bin/codesign verify

find string xref to "/usr/bin/codesign"

### x86_64

```
__text:00000001007C9280 55                                      push    rbp
__text:00000001007C9281 48 89 E5                                mov     rbp, rsp
__text:00000001007C9284 41 57                                   push    r15
__text:00000001007C9286 41 56                                   push    r14
__text:00000001007C9288 41 55                                   push    r13
__text:00000001007C928A 41 54                                   push    r12
__text:00000001007C928C 53                                      push    rbx
__text:00000001007C928D 48 81 EC 38 04 00 00                    sub     rsp, 438h
__text:00000001007C9294 4C 89 85 B8 FB FF FF                    mov     [rbp+var_448], r8
__text:00000001007C929B 48 89 8D B0 FB FF FF                    mov     [rbp+var_450], rcx
__text:00000001007C92A2 48 89 95 A8 FB FF FF                    mov     [rbp+var_458], rdx
__text:00000001007C92A9 41 89 F4                                mov     r12d, esi
__text:00000001007C92AC 48 89 FB                                mov     rbx, rdi
__text:00000001007C92AF 48 8B 05 7A 70 1C 00                    mov     rax, cs:___stack_chk_guard_ptr
__text:00000001007C92B6 48 8B 00                                mov     rax, [rax]
__text:00000001007C92B9 48 89 45 D0                             mov     [rbp+var_30], rax
__text:00000001007C92BD 48 8B 0D B4 83 27 00                    mov     rcx, cs:off_100A41678 ; "4C6364ACXT"
__text:00000001007C92C4 48 8D 15 C4 2A 15 00                    lea     rdx, aAnchorAppleGen_0 ; "=anchor apple generic and certificate l"...
__text:00000001007C92CB 45 31 F6                                xor     r14d, r14d
__text:00000001007C92CE 48 8D BD D0 FB FF FF                    lea     rdi, [rbp+__str] ; __str
__text:00000001007C92D5 BE 00 04 00 00                          mov     esi, 400h       ; __size
__text:00000001007C92DA 31 C0                                   xor     eax, eax
__text:00000001007C92DC E8 73 CA 01 00                          call    _snprintf
__text:00000001007C92E1 48 C7 85 C0 FB FF FF 00+                mov     [rbp+staticCode], 0
__text:00000001007C92E1 00 00 00
__text:00000001007C92EC 48 8D 3D 8A 2A 15 00                    lea     rdi, __file     ; "/usr/bin/codesign"
__text:00000001007C92F3 BE 01 00 00 00                          mov     esi, 1          ; int
__text:00000001007C92F8 E8 3F C4 01 00                          call    _access
__text:00000001007C92FD 85 C0                                   test    eax, eax
__text:00000001007C92FF 74 29                                   jz      short loc_1007C932A
```

opcode

```
55 48 89 E5 41 57 41 56 41 55 41 54 53 48 81 EC
38 04 00 00 4C 89 85 B8 FB FF FF 48 89 8D B0 FB
FF FF 48 89 95 A8 FB FF FF 41 89 F4 48 89 FB 48
8B 05 7A 70 1C 00 48 8B 00 48 89 45 D0 48 8B 0D
```

patch

```
6A 01 58 C3
```

after

```
__text:00000001007C9280                         sub_1007C9280   proc near               ; CODE XREF: sub_100175930+28F↑p
__text:00000001007C9280 6A 01                                   push    1
__text:00000001007C9282 58                                      pop     rax
__text:00000001007C9283 C3                                      retn
__text:00000001007C9283                         sub_1007C9280   endp
```

### arm64

```
__text:00000001007B3A14 FA 67 BB A9                             STP             X26, X25, [SP,#-0x10+var_40]!
__text:00000001007B3A18 F8 5F 01 A9                             STP             X24, X23, [SP,#0x40+var_30]
__text:00000001007B3A1C F6 57 02 A9                             STP             X22, X21, [SP,#0x40+var_20]
__text:00000001007B3A20 F4 4F 03 A9                             STP             X20, X19, [SP,#0x40+var_10]
__text:00000001007B3A24 FD 7B 04 A9                             STP             X29, X30, [SP,#0x40+var_s0]
__text:00000001007B3A28 FD 03 01 91                             ADD             X29, SP, #0x40
__text:00000001007B3A2C FF 43 11 D1                             SUB             SP, SP, #0x450
__text:00000001007B3A30 F6 03 04 AA                             MOV             X22, X4
__text:00000001007B3A34 F7 03 03 AA                             MOV             X23, X3
__text:00000001007B3A38 F4 03 02 AA                             MOV             X20, X2
__text:00000001007B3A3C F5 03 01 AA                             MOV             X21, X1
__text:00000001007B3A40 F3 03 00 AA                             MOV             X19, X0
__text:00000001007B3A44 C8 0D 00 B0                             ADRP            X8, #___stack_chk_guard_ptr@PAGE
__text:00000001007B3A48 08 6D 41 F9                             LDR             X8, [X8,#___stack_chk_guard_ptr@PAGEOFF]
__text:00000001007B3A4C 08 01 40 F9                             LDR             X8, [X8]
__text:00000001007B3A50 A8 83 1B F8                             STUR            X8, [X29,#var_48]
__text:00000001007B3A54 48 13 00 D0                             ADRP            X8, #off_100A1DB18@PAGE ; "4C6364ACXT"
__text:00000001007B3A58 08 8D 45 F9                             LDR             X8, [X8,#off_100A1DB18@PAGEOFF] ; "4C6364ACXT"
__text:00000001007B3A5C E8 03 00 F9                             STR             X8, [SP,#0x490+var_490]
__text:00000001007B3A60 C2 0C 00 90 42 18 34 91                 ADRL            X2, aAnchorAppleGen_0 ; "=anchor apple generic and certificate l"...
__text:00000001007B3A68 E0 23 01 91                             ADD             X0, SP, #0x490+__str ; __str
__text:00000001007B3A6C 01 80 80 52                             MOV             W1, #0x400 ; __size
__text:00000001007B3A70 AA 76 00 94                             BL              _snprintf
__text:00000001007B3A74 FF 1F 00 F9                             STR             XZR, [SP,#0x490+staticCode]
__text:00000001007B3A78 C0 0C 00 90 00 D0 33 91                 ADRL            X0, aUsrBinCodesign ; "/usr/bin/codesign"
__text:00000001007B3A80 21 00 80 52                             MOV             W1, #1  ; int
__text:00000001007B3A84 93 73 00 94                             BL              _access
__text:00000001007B3A88 E0 01 00 34                             CBZ             W0, loc_1007B3AC4
__text:00000001007B3A8C
```

opcode

```
FA 67 BB A9 F8 5F 01 A9 F6 57 02 A9 F4 4F 03 A9
FD 7B 04 A9 FD 03 01 91 FF 43 11 D1 F6 03 04 AA
F7 03 03 AA F4 03 02 AA F5 03 01 AA F3 03 00 AA
C8 0D 00 B0 08 6D 41 F9 08 01 40 F9 A8 83 1B F8
```

patch

```
20 00 80 D2 C0 03 5F D6
```

after

```
__text:00000001007B3A14                         sub_1007B3A14                           ; CODE XREF: sub_10018297C+2D4↑p
__text:00000001007B3A14 20 00 80 D2                             MOV             X0, #1
__text:00000001007B3A18 C0 03 5F D6                             RET
__text:00000001007B3A18                         ; End of function sub_1007B3A14
```

## 3. add write licenses.json

1. use step 2 code space (0x1007C9284) add shellcode write license data

### x86_64

opcode

```
41 57 41 56 41 55 41 54 53 48 81 EC 38 04 00 00
4C 89 85 B8 FB FF FF 48 89 8D B0 FB FF FF 48 89
95 A8 FB FF FF 41 89 F4 48 89 FB 48 8B 05 7A 70
1C 00 48 8B 00 48 89 45 D0 48 8B 0D B4 83 27 00
48 8D 15 C4 2A 15 00 45 31 F6 48 8D BD D0 FB FF
FF BE 00 04 00 00 31 C0 E8 73 CA 01 00 48 C7 85
C0 FB FF FF 00 00 00 00 48 8D 3D 8A 2A 15 00 BE
01 00 00 00 E8 3F C4 01 00 85 C0 74 29 48 8B 05
28 70 1C 00 48 8B 00 48 3B 45 D0 0F 85 58 02 00
00 44 89 F0 48 81 C4 38 04 00 00 5B 41 5C 41 5D
41 5E 41 5F 5D C3 48 8B 05 4F 70 1C 00 48 8B 38
48 8D 35 42 2A 15 00 BA 11 00 00 00 31 C9 E8 FD
82 01 00 48 85 C0 74 B5 49 89 C7 48 89 C7 E8 BD
82 01 00 49 89 C5 4C 89 FF E8 BC 81 01 00 4D 85
ED 74 9A 48 8D 95 C0 FB FF FF 4C 89 EF 31 F6 E8
A4 86 01 00 49 89 DF 89 C3 4C 89 EF E8 99 81 01
00 85 DB 0F 85 74 FF FF FF 4C 8B
```

patch

```
55 48 89 E5 53 56 52 48 8D 3D 3F 00 00 00 48 8D
35 65 00 00 00 E8 24 C6 01 00 49 89 C6 48 8D 3D
58 00 00 00 BE 8E 00 00 00 BA 01 00 00 00 4C 89
F1 E8 62 C6 01 00 4C 89 F7 E8 E8 C5 01 00 4C 89
F7 E8 C2 C5 01 00 5A 5E E9 84 68 F9 FF 2F 4C 69
62 72 61 72 79 2F 50 72 65 66 65 72 65 6E 63 65
73 2F 50 61 72 61 6C 6C 65 6C 73 2F 6C 69 63 65
6E 73 65 73 2E 6A 73 6F 6E 00 77 00 7B 22 6C 69
63 65 6E 73 65 22 3A 22 7B 5C 22 70 72 6F 64 75
63 74 5F 76 65 72 73 69 6F 6E 5C 22 3A 5C 22 31
38 2E 2A 5C 22 2C 5C 22 65 64 69 74 69 6F 6E 5C
22 3A 32 2C 5C 22 70 6C 61 74 66 6F 72 6D 5C 22
3A 33 2C 5C 22 70 72 6F 64 75 63 74 5C 22 3A 37
2C 5C 22 6F 66 66 6C 69 6E 65 5C 22 3A 74 72 75
65 2C 5C 22 63 70 75 5F 6C 69 6D 69 74 5C 22 3A
33 32 2C 5C 22 72 61 6D 5F 6C 69 6D 69 74 5C 22
3A 31 33 31 30 37 32 7D 22 7D 00
```

after

```
__text:00000001007C9284                         write_fake_lic  proc near               ; CODE XREF: __text:InitFunc_0↑j
__text:00000001007C9284                                                                 ; __text:loc_10075FB50↑j
__text:00000001007C9284 55                                      push    rbp
__text:00000001007C9285 48 89 E5                                mov     rbp, rsp
__text:00000001007C9288 53                                      push    rbx
__text:00000001007C9289 56                                      push    rsi
__text:00000001007C928A 52                                      push    rdx
__text:00000001007C928B 48 8D 3D 3F 00 00 00                    lea     rdi, aLibraryPrefere_2 ; "/Library/Preferences/Parallels/licenses"...
__text:00000001007C9292 48 8D 35 65 00 00 00                    lea     rsi, aW_1       ; "w"
__text:00000001007C9299 E8 24 C6 01 00                          call    _fopen
__text:00000001007C929E 49 89 C6                                mov     r14, rax
__text:00000001007C92A1 48 8D 3D 58 00 00 00                    lea     rdi, aLicenseProduct_0 ; "{\"license\":\"{\\\"product_version\\\""...
__text:00000001007C92A8 BE 8E 00 00 00                          mov     esi, 8Eh        ; size_t
__text:00000001007C92AD BA 01 00 00 00                          mov     edx, 1          ; size_t
__text:00000001007C92B2 4C 89 F1                                mov     rcx, r14        ; FILE *
__text:00000001007C92B5 E8 62 C6 01 00                          call    _fwrite
__text:00000001007C92BA 4C 89 F7                                mov     rdi, r14        ; FILE *
__text:00000001007C92BD E8 E8 C5 01 00                          call    _fflush
__text:00000001007C92C2 4C 89 F7                                mov     rdi, r14        ; FILE *
__text:00000001007C92C5 E8 C2 C5 01 00                          call    _fclose
__text:00000001007C92CA 5A                                      pop     rdx
__text:00000001007C92CB 5E                                      pop     rsi
__text:00000001007C92CC E9 84 68 F9 FF                          jmp     sub_10075FB55
__text:00000001007C92CC                         write_fake_lic  endp
__text:00000001007C92CC
__text:00000001007C92CC                         ; ---------------------------------------------------------------------------
__text:00000001007C92D1                         ; const char aLibraryPrefere_2[]
__text:00000001007C92D1 2F 4C 69 62 72 61 72 79+aLibraryPrefere_2 db '/Library/Preferences/Parallels/licenses.json',0
__text:00000001007C92D1 2F 50 72 65 66 65 72 65+                                        ; DATA XREF: write_fake_lic+7↑o
__text:00000001007C92FE                         ; const char aW_1[]
__text:00000001007C92FE 77 00                   aW_1            db 'w',0                ; DATA XREF: write_fake_lic+E↑o
__text:00000001007C9300 7B 22 6C 69 63 65 6E 73+aLicenseProduct_0 db '{"license":"{\"product_version\":\"18.*\",\"edition\":2,\"platfor'
__text:00000001007C9300 65 22 3A 22 7B 5C 22 70+                                        ; CODE XREF: __text:00000001007C9487↓j
__text:00000001007C9300 72 6F 64 75 63 74 5F 76+                                        ; __text:00000001007C9568↓j
__text:00000001007C9300 65 72 73 69 6F 6E 5C 22+                                        ; DATA XREF: ...
__text:00000001007C9300 3A 5C 22 31 38 2E 2A 5C+                db 'm\":3,\"product\":7,\"offline\":true,\"cpu_limit\":32,\"ram_limit'
__text:00000001007C9300 22 2C 5C 22 65 64 69 74+                db '\":131072}"}',0
```

### arm64

opcode

```
```

patch

```
```

after

```
```

2. find string xref "licenses.json"

### x86_64

```
__text:000000010075FB50 55                                      push    rbp
__text:000000010075FB51 48 89 E5                                mov     rbp, rsp
__text:000000010075FB54 53                                      push    rbx
__text:000000010075FB55 48 83 EC 28                             sub     rsp, 28h
__text:000000010075FB59 48 89 FB                                mov     rbx, rdi
__text:000000010075FB5C 48 8D 3D CD F9 10 00                    lea     rdi, a12        ; "%1/%2"
__text:000000010075FB63 BE 05 00 00 00                          mov     esi, 5          ; char *
__text:000000010075FB68 E8 09 3D 08 00                          call    __ZN7QString16fromAscii_helperEPKci ; QString::fromAscii_helper(char const*,int)
__text:000000010075FB6D 48 89 45 E0                             mov     [rbp+var_20], rax
__text:000000010075FB71 48 8D 7D E8                             lea     rdi, [rbp+var_18]
__text:000000010075FB75 E8 26 F4 FF FF                          call    sub_10075EFA0
__text:000000010075FB7A 48 8D 7D D8                             lea     rdi, [rbp+var_28]
__text:000000010075FB7E 48 8D 75 E0                             lea     rsi, [rbp+var_20]
__text:000000010075FB82 48 8D 55 E8                             lea     rdx, [rbp+var_18]
__text:000000010075FB86 31 C9                                   xor     ecx, ecx
__text:000000010075FB88 41 B8 20 00 00 00                       mov     r8d, 20h ; ' '
__text:000000010075FB8E E8 5B 55 08 00                          call    __ZNK7QString3argERKS_i5QChar ; QString::arg(QString const&,int,QChar)
__text:000000010075FB93 48 8D 3D 89 83 19 00                    lea     rdi, aLicensesJson ; "licenses.json"
__text:000000010075FB9A BE 0D 00 00 00                          mov     esi, 0Dh        ; char *
__text:000000010075FB9F E8 D2 3C 08 00                          call    __ZN7QString16fromAscii_helperEPKci ; QString::fromAscii_helper(char const*,int)
__text:000000010075FBA4 48 89 45 F0                             mov     [rbp+var_10], rax
__text:000000010075FBA8 48 8D 75 D8                             lea     rsi, [rbp+var_28]
__text:000000010075FBAC 48 8D 55 F0                             lea     rdx, [rbp+var_10]
__text:000000010075FBB0 48 89 DF                                mov     rdi, rbx
__text:000000010075FBB3 31 C9                                   xor     ecx, ecx
__text:000000010075FBB5 41 B8 20 00 00 00                       mov     r8d, 20h ; ' '
__text:000000010075FBBB E8 2E 55 08 00                          call    __ZNK7QString3argERKS_i5QChar ; QString::arg(QString const&,int,QChar)
__text:000000010075FBC0 48 8B 7D F0                             mov     rdi, [rbp+var_10]
__text:000000010075FBC4 8B 07                                   mov     eax, [rdi]
__text:000000010075FBC6 83 F8 FF                                cmp     eax, 0FFFFFFFFh
__text:000000010075FBC9 74 1D                                   jz      short loc_10075FBE8
__text:000000010075FBCB 85 C0                                   test    eax, eax
__text:000000010075FBCD 74 0A                                   jz      short loc_10075FBD9
__text:000000010075FBCF F0 83 2F 01                             lock sub dword ptr [rdi], 1
__text:000000010075FBD3 75 13                                   jnz     short loc_10075FBE8
__text:000000010075FBD5 48 8B 7D F0                             mov     rdi, [rbp+var_10]
```

opcode

```
55 48 89 E5 53 48 83 EC 28 48 89 FB 48 8D 3D CD
F9 10 00 BE 05 00 00 00 E8 09 3D 08 00 48 89 45
E0 48 8D 7D E8 E8 26 F4 FF FF 48 8D 7D D8 48 8D
75 E0 48 8D 55 E8 31 C9 41 B8 20 00 00 00 E8 5B
```

patch

```
E9 2F 97 06 00
```

after

```
__text:000000010075FB50 E9 2F 97 06 00                          jmp     write_fake_lic
```

### arm64


