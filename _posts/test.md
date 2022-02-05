---
layout: post
title: 'TEST'
date: 2022-01-21 18:17:16'
pin: true
categories: story work osvc isp
tags: PánCoDětemOpravovalYoutube
---
== Nastavenie na Zasuvkach ==

Pripojime sa cez SSH ( je to rychlejsie a prehladnejsie ako ten web )

* 2 Network -&gt; 8 SNMP
* 1 Settings -&gt; vypneme v1 zapneme v3
 ------- Settings --------------------------------------------------------------
 
      1- SNMPv1 Access : Disabled
      2- SNMPv3 Access : Enabled
      3- Accept Changes: 
*upravime SNMPv3 profile 1 settings
 ------- SNMPv3 User Profile 1 -------------------------------------------------
 
         User Profile Summary
         #  User Name               Authentication             Privacy
         -----------------------------------------------------------------------
         1  pdu_adm                 MD5                        DES
         2  apc snmp profile2       No Authentication          No Privacy
         3  apc snmp profile3       No Authentication          No Privacy
         4  apc snmp profile4       No Authentication          No Privacy
 
      1- User Name            : pdu_adm
      2- Authentication Phrase: IHgMVoSyaHG2aNVQHiOG
      3- Encryption Phrase    : QjOsaKjVu9eGjVN9aWzv
      4- Access Type          : Auth/Priv
      5- Accept Changes       : 
V tomto bode je dolezite spravne nastavit Auth Phrase pre autentifikaciu a Encrypt Phrase pre sifrovanie. Ak sa zada moc kratky string tak to hadze BAD DATA

* upravime SNMPv3 access control pre profile 1 (povolime pre ip 62.44.0.53
 ------- SNMPv3 Access Control 1 -----------------------------------------------
 
         Access Control Summary
         #  User Name               Access         NMS IP/Name
         -----------------------------------------------------------------------
         1  pdu_adm                 Enabled        62.44.0.53
         2  apc snmp profile2       Disabled       0.0.0.0
         3  apc snmp profile3       Disabled       0.0.0.0
         4  apc snmp profile4       Disabled       0.0.0.0 
 
      1- Access        : Enabled 
      2- User Name     : pdu_adm
      3- NMS IP/Name   : 62.44.0.53
      4- Accept Changes: 

Pozor na User Name při schvalování - po zadání IP a schválení ho zásuvky nastaví na &quot;apc snmp profile2&quot;!

* spravne upravime trap receiver 1
 ------- Trap Receiver 1 -------------------------------------------------------
 
         Trap Receiver Summary
         #  Community        Trap Type  Generation  Auth Traps  Receiver NMS IP
         -----------------------------------------------------------------------
         1  apc_control      SNMPv3     Enabled     Enabled     0.0.0.0
         2  public           SNMPv1     Enabled     Enabled     0.0.0.0
         3  public           SNMPv1     Enabled     Enabled     0.0.0.0
         4  public           SNMPv1     Enabled     Enabled     0.0.0.0
         5  public           SNMPv1     Enabled     Enabled     0.0.0.0
         6  public           SNMPv1     Enabled     Enabled     0.0.0.0
 
      1- Community Name      : apc_control
      2- Trap Type           : SNMPv3
      3- Trap Generation     : Enabled 
      4- Authentication Traps: Enabled 
      5- Receiver NMS IP/Name: 0.0.0.0
      6- User Name           : pdu_adm
      7- Accept Changes      : 
V tomto je dolezite mat v 6-User name vybraty nas profil a usera ktoreho sme vytvarali

Treba byt trpezlivy, zasuvkam niekedy trva aj 2 minuty nez si uvedomia zmeny a zacne pristup fungovat.

== Prakticke vyuzitie ==

Specifikaciu MIB OID najdeme na http://www.oidview.com/mibs/318/PowerNet-MIB.html
pripadne aj s popisom na http://support.ipmonitor.com/mibs/POWERNET-MIB/oids.aspx
Oznacenie MIB je PowerNET.

Vytazenie zasuviek v Amperech
 1.3.6.1.4.1.318.1.1.12.2.3.1.1.2

Ziskanie meno zasuvky
 1.3.6.1.4.1.318.1.1.4.5.2.1.3.&lt;cislo outletu&gt;

Zmena mena pre zasuvku
 1.3.6.1.4.1.318.1.1.4.5.2.1.3.&lt;cislo zasuvky&gt; , maximalne 20 znakov, typ udaju STRING

Zistenie stavu zasuvky
 1.3.6.1.4.1.318.1.1.12.3.5.1.1.4.&lt;cislo zasuvky&gt; , 1-Zapnuta, 2-Vypnuta 

Vypnutie zasuvky cislo 5
 snmpset -v 3 -c apc_control -A IHgMVoSyaHG2aNVQHiOG -l authPriv -u pdu_adm -x DES -X QjOsaKjVu9eGjVN9aWzv 195.250.144.134 1.3.6.1.4.1.318.1.1.12.3.3.1.1.4.5 i 2
 
 # -v verzia protokolu
 # -c comunity nastavena nami na zasuvkach
 # -A autentifikacna fraza nastavena nami na zasuvkach
 # -l authPriv (autentifikacia zapnuta, sifrovanie zapnute)
 # -u username
 # -x sposob sifrovania
 # -X sifrovacia fraza, nastavena nami na zasuvkach
 #
 # OID - cislo pre ovladanie zasuvky (1.3.6.1.4.1.318.1.1.12.3.3.1.1.4.) na konci je 5 (cislo zasuvky)
 # i znamena ze posielame hodnotu integer
 # 2 znamena vypnut ( 1-zapnut, 2-vypnut, 3-reboot, 4-delayed on, 5-delayed off, 6-delayed reboot, 7-cancel command )</text>
      <sha1>n3pdwsyventl6xfwukpafwag46q6hre</sha1>
    </revision>
  </page>
</mediawiki>