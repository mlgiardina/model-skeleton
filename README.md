This folder structure should be suitable for starting a project that uses a database:

* Fork this repo
* Clone this repo
* `rake generate:migration <NAME>` to create a migration (Don't include the `<` `>` in your name, it should also start with a capital)
* `rake db:migrate` to run the migration and update the database
* Create models in lib that subclass `ActiveRecord::Base`
* ... ?
* Profit


## Rundown

```
.
├── Gemfile             # Details which gems are required by the project
├── README.md           # This file
├── Rakefile            # Defines `rake generate:migration` and `db:migrate`
├── config
│   └── database.yml    # Defines the database config (e.g. name of file)
├── console.rb          # `ruby console.rb` starts `pry` with models loaded
├── db
│   ├── dev.sqlite3     # Default location of the database file
│   ├── migrate         # Folder containing generated migrations
│   └── setup.rb        # `require`ing this file sets up the db connection
└── lib                 # Your ruby code (models, etc.) should go here
    └── all.rb          # Require this file to auto-require _all_ `.rb` files in `lib`
```

1. There are 51 users. I found this by typing `User.count`.
2.  The 5 most expensive items are:

	|          Item         | Price |
	|:---------------------:|:-----:|
	|  Small Cotton Gloves  |  9984 |
	| Small Wooden Computer |  9859 |
	| Awesome Granite Pants |  9790 |
	|    Sleek Wooden Hat   |  9390 |
	|  Ergonomic Steel Car  |  9341 |
I found this information by typing `Item.order('price').last(5)`.
3. The cheapest book is called Ergonomic Granite Chair (1496). I found this by typing `Item.order('price').where('category LIKE ?', '%book%').first`.
4. Corinne Little lives at "6439 Zetta Hills, Willmouth, WY." I found this by first typing `Address.find_by street: "6439 Zetta Hills"`. This showed me that the `user_id` was 40. Then I typed `User.where(user_id: 40)`, which showed me that she also lives at "54369 Wolff Forges, Lake Byron, CA 31587.
5. First, I typed `User.where(last_name: "Mitchell")` to learn that Virginie Mitchell's user id is 39. Then, I typed `Address.where(user_id: 39)` to learn that the id of the file I needed to change in the Address model is 41. Finally, I typed `Address.update(41, city: "NY", zip: 10108)` to update the address.
6. It would cost 46,477 to buy one of each tool. I found this by typing `Item.where("category LIKE ?", "%tool%").sum("price")`.
7. We sold 2125 items. I found this by typing `Order.sum("quantity")`.
8. It would cost 1,081,532 to buy every book. I found this by typing `Order.joins("JOIN items ON items.id = orders.item_id AND category LIKE '%book%'").sum("price * quantity")`.
9. I inserted myself into the users table by typing `User.find_or_create_by(first_name: "Matthew", last_name: "Giardina", email: "mlgiardina@me.com")`. Then, I created an order for myself by typing `Order.find_or_create_by(user_id: 51, item_id: 23, quantity: 100)`.