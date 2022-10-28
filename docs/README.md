# Sticker Collection Entity-Relationship Diagram

This diagram uses DBML (database markup language) and was implemented using [dbdiagram.io](https://dbdiagram.io/)

![Sticker Collection ER Diagram](./Sticker%20Collection%20ER%20Diagram.png)

```DBML
Table albums {
  id int [pk, increment]
  name varchar
  collection varchar
  publication_year int
  created_at datetime [default: `now()`]
  updated_at datetime [default: `now()`]

  Indexes {
    id [unique]
    (name) [name:'album_name']
    (collection) [name:'album_collection']
  }
}

Table sticker_type {
  id int [pk, increment]
  album_id int [not null, ref: > albums.id]
  name varchar [not null]
  description varchar
  created_at datetime [default: `now()`]
  updated_at datetime [default: `now()`]
}

Table stickers {
  id int [pk, increment]
  album_id int [not null, ref: > albums.id]
  type_id int [not null, ref: > sticker_type.id]
  code varchar
  group varchar
  page int
  type varchar [note: 'E.g.: regular, extra']
  created_at datetime [default: `now()`]
  updated_at datetime [default: `now()`]

  Indexes {
    id [unique]
    (code) [name:'sticker_code']
    (group) [name:'sticker_group']
  }
}

Table users as U {
  id int [pk, increment] // auto-increment
  full_name varchar
  email varchar
  contact_info varchar
  created_at datetime [default: `now()`]
  updated_at datetime [default: `now()`]
}

Table user_stickers {
  id int [pk, increment]
  user_id int [not null, ref: > users.id]
  album_id int [not null, ref: > albums.id]
  sticker_id int [not null, ref: > stickers.id, note: 'Redundant but kept for performance']
  quantity int [not null]
  available_for_trading int [not null, note: 'Quantity availabe for trading']
  available_for_selling int [not null, note: 'Quantity availabe for selling']

  created_at datetime [default: `now()`]
  updated_at datetime [default: `now()`]
}

// Enum for 'products' table below
Enum trading_status {
  requested
  accepted
  declined [note: 'less than 20'] // add column note
  withdraw [note: 'When the user that requested withdraws the order']
}

Table trading_matches {
  id int [pk, increment]
  requester_id int [not null, ref: > users.id]
  asked_sticker int [not null, ref: > user_stickers.id]
  status trading_status
  created_at datetime [default: `now()`]
  updated_at datetime [default: `now()`]
}
```
