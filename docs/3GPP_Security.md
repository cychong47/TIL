3GPP Security

3GPP Security

NAS Security, AS Security
=========================

보호하는 메시지/구간에 따라 구분

NAS
---

-	UE-MME 간

AS
--

-	UE-eNB 간
-	SRB를 통해 전달되는 RRC와 DRB를 통해 전달되는 Bearer data에 대한 보안
-	RRC는 Integrity & Ciphering(순서 중요)
-	Bearer는 Ciphering Only
-	Algorithm은 UE->MME->eNB로 전달되는 Attach Accept 메시지에 담긴 단말이 지원하는 algorithm 중에서 eNB 가 선택(최초 UE->MME의 Attach Request에 단말의 capable algorithm list가 전달되어 MME에 의해 NAS용 알고리즘이 선택됨)

Key
===

Key는 UE와 MME가 알고 있는 최초 root key인 K(LTE Key)에서 모든 키를 만들어 냄. UE의 경우 USIM에 저장

Key hierarchy
-------------

```
K(LTE Key)
    USIM an MME
    CK,IK
        K_ASME
            K_eNB
                K_RRCint, K_RRCenc, K_UPenc
```

K_eNB는 MME와 UE가 동일 key derivation function을 이용하여 root key로부터 만들어낸 K_ASME로부터 만들어 냄. eNB는 별도로 Key를 생성하지 않고 MME로부터 단말과의 AS security에 사용할 key를 전달 받음

Message flow
============

-	Attach Request : UE -> MME
-	Attach Accept : MME -> eNB
-	Security Mode Command : eNB -> UE, Integrity Only
-	Security Mode Confirm : UE -> eNB, Integrity Only
-	이 후 모든 RRC와 User bearer는 각각 Integrity & Ciphering, Ciphering되어 전달됨

Integrity & Ciphering
=====================

Operation Order
---------------

IPsec과 달리 Integrity를 먼저 수행하여 MAC-I를 생성한 후 원본데이터와 MAC-I를 포함한 부분에 Ciphering을 적용한다. (IPsec의 경우 먼저 ciphering을 적용한 후 뒤에 ICV를 붙임)

Input data
----------

-	Count : 32bit PDCP count
-	Message
-	Direction : 0 for DL, 1 for UL
-	Bearer ID : 5 bit
-	Key ; 128bit AS key

용어
====

| 약어  | 풀이                                      |
|-------|-------------------------------------------|
| NAS   | Non Access Stratum                        |
| AS    | Access Stratum                            |
| RRC   | Radio Resource Control                    |
| SRB   | Signaling Radio Bearer                    |
| DRB   | Data Radio Bearer                         |
| ASME  | Access Security Management Entity         |
| EPS   | Evolved Packet System                     |
| AKA   | Authentication and Key Agreement          |
| EEA   | EPS Encryption Algorithm                  |
| EIA   | EPS Integrity Algorithm                   |
| MAC-I | Message Authentication Code for Integrity |
