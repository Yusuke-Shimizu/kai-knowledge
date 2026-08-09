---
doc_id: DEC-008b
doc_type: decision
title: 商品検索は OpenSearch Service に切り出す
date: 2026-06-18
status: active
supersedes: DEC-008a
superseded_by: null
decided_by: アーキテクト
owner: 設計リード
review_by: 2026-12-31
topic: datastore
---

# DEC-008b: 商品検索は OpenSearch Service に切り出す

**決定日**: 2026-06-18 / **状態**: active

## 決定
商品検索を Amazon OpenSearch Service に切り出す。Aurora は主データストアのまま
（DEC-002 は変更しない）で、検索用の索引を非同期に追従させる。

## 理由
- 大口テナントの実データが想定の 10 倍（数十万件）で、`ILIKE` の部分一致が
  秒単位まで劣化した
- 表記ゆれ・かな漢字の揺れに `ILIKE` では対応できず、要件を満たせないと判明した

## 経緯
DEC-008a（Aurora の ILIKE 検索）を見直し、本決定で置換した。
索引の追従は DEC-003b の SQS + Lambda に乗せる。
