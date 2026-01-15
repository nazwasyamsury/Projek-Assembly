.model small
.stack 100h

.data
    pin db '5'

    msgPin   db 13,10,"Masukkan PIN: $"
    msgSalah db 13,10,"PIN SALAH!$"

    menu db 13,10,"=== M-BANK ===",13,10
         db "1. Cek Saldo",13,10
         db "2. Setor (x1000)",13,10
         db "3. Tarik (x1000)",13,10
         db "4. Keluar",13,10
         db "Pilih [1-4]: $"

    msgSaldo db 13,10,"Saldo Anda: $"
    msgSetor db 13,10,"Setor (1=1000): $"
    msgTarik db 13,10,"Tarik (1=1000): $"
    msgGagal db 13,10,"Saldo tidak cukup!$"
    msgExit  db 13,10,"Terima kasih.$"

    saldo dw 5000       ; saldo awal = 5000

.code
main proc
    mov ax, @data
    mov ds, ax

; ===== LOGIN =====
login:
    mov ah, 09h
    lea dx, msgPin
    int 21h

    mov ah, 01h
    int 21h

    cmp al, pin
    jne salah
    jmp menu_utama

salah:
    mov ah, 09h
    lea dx, msgSalah
    int 21h
    jmp login

; ===== MENU =====
menu_utama:
    mov ah, 09h
    lea dx, menu
    int 21h

    mov ah, 01h
    int 21h

    cmp al, '1'
    je cek_saldo
    cmp al, '2'
    je setor
    cmp al, '3'
    je tarik
    cmp al, '4'
    je keluar
    jmp menu_utama

; ===== CEK SALDO =====
cek_saldo:
    mov ah, 09h
    lea dx, msgSaldo
    int 21h

    mov ax, saldo
    call tampil_angka
    jmp menu_utama

; ===== SETOR =====
setor:
    mov ah, 09h
    lea dx, msgSetor
    int 21h

    mov ah, 01h
    int 21h
    sub al, '0'     ; 1 digit

    mov ah, 0
    mov bx, 1000
    mul bx          ; AX = input × 1000

    add saldo, ax
    jmp menu_utama

; ===== TARIK =====
tarik:
    mov ah, 09h
    lea dx, msgTarik
    int 21h

    mov ah, 01h
    int 21h
    sub al, '0'

    mov ah, 0
    mov bx, 1000
    mul bx          ; AX = input × 1000

    cmp saldo, ax
    jb gagal

    sub saldo, ax
    jmp menu_utama

gagal:
    mov ah, 09h
    lea dx, msgGagal
    int 21h
    jmp menu_utama

; ===== KELUAR =====
keluar:
    mov ah, 09h
    lea dx, msgExit
    int 21h

    mov ah, 4Ch
    int 21h
main endp

; ===== TAMPIL ANGKA WORD =====
tampil_angka proc
    push ax
    push bx
    push cx
    push dx

    mov bx, 10
    xor cx, cx

ulang:
    xor dx, dx
    div bx
    push dx
    inc cx
    cmp ax, 0
    jne ulang

cetak:
    pop dx
    add dl, '0'
    mov ah, 02h
    int 21h
    loop cetak

    pop dx
    pop cx
    pop bx
    pop ax
    ret
tampil_angka endp

end main
