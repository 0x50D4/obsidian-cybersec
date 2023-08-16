```
    1159:	55                   	push   %rbp
    115a:	48 89 e5             	mov    %rsp,%rbp
    115d:	53                   	push   %rbx
    115e:	48 83 ec 28          	sub    $0x28,%rsp
    1162:	89 7d dc             	mov    %edi,-0x24(%rbp)
    1165:	48 89 75 d0          	mov    %rsi,-0x30(%rbp)
    1169:	83 7d dc 02          	cmpl   $0x2,-0x24(%rbp)
    116d:	0f 85 b4 00 00 00    	jne    1227 <main+0xce>
    1173:	48 8b 45 d0          	mov    -0x30(%rbp),%rax
    1177:	48 83 c0 08          	add    $0x8,%rax
    117b:	48 8b 00             	mov    (%rax),%rax
    117e:	48 89 c6             	mov    %rax,%rsi
    1181:	48 8d 05 7c 0e 00 00 	lea    0xe7c(%rip),%rax        # 2004 <_IO_stdin_used+0x4>
    1188:	48 89 c7             	mov    %rax,%rdi
    118b:	b8 00 00 00 00       	mov    $0x0,%eax
    1190:	e8 bb fe ff ff       	call   1050 <printf@plt>
    1195:	c7 45 e8 00 00 00 00 	movl   $0x0,-0x18(%rbp)
    119c:	c7 45 ec 00 00 00 00 	movl   $0x0,-0x14(%rbp)
    11a3:	eb 20                	jmp    11c5 <main+0x6c>
    11a5:	48 8b 45 d0          	mov    -0x30(%rbp),%rax
    11a9:	48 83 c0 08          	add    $0x8,%rax
    11ad:	48 8b 10             	mov    (%rax),%rdx
    11b0:	8b 45 ec             	mov    -0x14(%rbp),%eax
    11b3:	48 98                	cltq
    11b5:	48 01 d0             	add    %rdx,%rax
    11b8:	0f b6 00             	movzbl (%rax),%eax
    11bb:	0f be c0             	movsbl %al,%eax
    11be:	01 45 e8             	add    %eax,-0x18(%rbp)
    11c1:	83 45 ec 01          	addl   $0x1,-0x14(%rbp)
    11c5:	8b 45 ec             	mov    -0x14(%rbp),%eax
    11c8:	48 63 d8             	movslq %eax,%rbx
    11cb:	48 8b 45 d0          	mov    -0x30(%rbp),%rax
    11cf:	48 83 c0 08          	add    $0x8,%rax
    11d3:	48 8b 00             	mov    (%rax),%rax
    11d6:	48 89 c7             	mov    %rax,%rdi
    11d9:	e8 62 fe ff ff       	call   1040 <strlen@plt>
    11de:	48 39 c3             	cmp    %rax,%rbx
    11e1:	72 c2                	jb     11a5 <main+0x4c>
    11e3:	8b 45 e8             	mov    -0x18(%rbp),%eax
    11e6:	89 c6                	mov    %eax,%esi
    11e8:	48 8d 05 2b 0e 00 00 	lea    0xe2b(%rip),%rax        # 201a <_IO_stdin_used+0x1a>
    11ef:	48 89 c7             	mov    %rax,%rdi
    11f2:	b8 00 00 00 00       	mov    $0x0,%eax
    11f7:	e8 54 fe ff ff       	call   1050 <printf@plt>
    11fc:	81 7d e8 94 03 00 00 	cmpl   $0x394,-0x18(%rbp)
    1203:	75 11                	jne    1217 <main+0xbd>
    1205:	48 8d 05 19 0e 00 00 	lea    0xe19(%rip),%rax        # 2025 <_IO_stdin_used+0x25>
    120c:	48 89 c7             	mov    %rax,%rdi
    120f:	e8 1c fe ff ff       	call   1030 <puts@plt>
    1214:	eb 20                	jmp    1236 <main+0xdd>
    1216:	48 8d 05 18 0e 00 00 	lea    0xe18(%rip),%rax        # 2035 <_IO_stdin_used+0x35>
    121d:	48 89 c7             	mov    %rax,%rdi
    1220:	e8 0b fe ff ff       	call   1030 <puts@plt>
    1225:	eb 0f                	jmp    1236 <main+0xdd>
    1227:	48 8d 05 0e 0e 00 00 	lea    0xe0e(%rip),%rax        # 203c <_IO_stdin_used+0x3c>
    122e:	48 89 c7             	mov    %rax,%rdi
    1231:	e8 fa fd ff ff       	call   1030 <puts@plt>
    1236:	b8 00 00 00 00       	mov    $0x0,%eax
    123b:	48 8b 5d f8          	mov    -0x8(%rbp),%rbx
    123f:	c9                   	leave
    1240:	c3                   	ret
```