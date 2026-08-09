---
doc_id: DEC-008a
doc_type: decision
title: 商品検索は Aurora の ILIKE 検索で始める
date: 2026-04-10
status: superseded
supersedes: null
superseded_by: DEC-008b
decided_by: アーキテクト
owner: 設計リード
review_by: 2026-12-31
topic: datastore
---

# DEC-008a: 商品検索は Aurora の ILIKE 検索で始める

**決定日**: 2026-04-10 / **状態**: superseded

## 決定
在庫画面の商品検索は、主データストア（DEC-002 の Aurora PostgreSQL）への
`ILIKE` 検索で実装する。検索専用のミドルウェアは置かない。

## 理由
- 初期の想定件数は 1 テナントあたり数万件で、部分一致でも応答は許容範囲に収まる
- 構成要素を増やさないほうが移行のリスクが小さい

## 注記
本決定は **DEC-008b により置換（superseded）**。
