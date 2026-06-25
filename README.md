# CS-360-Portfolio-Reflection

# Inventory App — CS 360 Portfolio Artifact

This repository contains the completed mobile application developed for CS 360, submitted as my Module Eight Journal portfolio artifact. The ZIP file included here (`Clayton_Cooper_InventoryApp_Project3.zip`) contains the final Android Studio project, including the finalized UI design (originally created in Project Two) and the completed, functional code (developed in Project Three).

## Reflection

**App requirements and goals**
This app is a mobile inventory management tool built for small business or personal use. The core user need was a simple, secure way to track items, categories, and stock quantities from a phone, with no internet connection required. To meet that need, the app supports user account creation and login, a dashboard for viewing and managing inventory, the ability to add new items, and an automatic low-stock SMS alert so the user knows immediately when an item runs out, even if they aren't looking at the app.

**Screens, features, and user-centered design**
The app has three main screens: a Login/Create Account screen, an inventory Dashboard, and an Add Item screen, plus a one-time SMS permission screen. The login screen keeps each user's inventory private and tied to their own account. The dashboard was designed around the most common task — scanning items and adjusting quantities — so it uses a simple list/grid layout with inline quantity editing and delete actions, rather than forcing the user to open a separate screen for every small change. The Add Item screen is a focused single-purpose form so users aren't distracted by unrelated controls while entering a new item. The SMS permission request is deferred to first login (not bundled into install) and the app continues to function fully if permission is denied, which respects user choice and follows Android best practices for runtime permissions. These choices were successful because they kept the most frequent actions (viewing and updating stock) fast and low-friction, while keeping less frequent actions (adding items, account creation) clearly separated.

**Coding approach**
I built the app in layers: first the data layer (a `DatabaseHelper` class wrapping SQLite, with dedicated CRUD methods for both the `users` and `inventory` tables), then the model (`InventoryItem`), then the UI layer (activities and an adapter for the inventory list). Keeping the database logic isolated in one helper class meant the activities only had to call simple methods like `addItem()`, `updateItemQuantity()`, or `deleteItem()` instead of writing SQL directly, which kept the UI code readable and made it easier to track down bugs. I also used SharedPreferences to track one-time state (whether SMS permission had already been requested) rather than re-prompting on every login. This separation-of-concerns approach is something I'll continue to use in future projects — keeping data access, business logic, and UI code in distinct classes makes an app easier to test, debug, and extend.

**Testing process**
I tested the app by running it on an Android emulator and manually walking through every user flow: creating an account, logging in with correct and incorrect credentials, adding items, editing quantities up and down (including down to zero to confirm the SMS alert fires), deleting items, and denying SMS permission to confirm the app still functioned normally afterward. I also tested edge cases like submitting empty fields and creating an account with a username that already existed. This process mattered because it surfaced real issues — for example, confirming that denying a permission didn't crash the app or block core functionality — that wouldn't have been obvious just from reading the code.

**Where I had to innovate**
The trickiest design challenge was the low-stock SMS alert. Android requires SEND_SMS to be requested at runtime, and a user can deny it at any point, so the app couldn't depend on that permission being available. I had to design the quantity-update logic so the SMS step was just one optional side effect of updating the database, wrapped in its own permission check, instead of something the core update logic depended on. That way the inventory feature works correctly whether or not the user grants SMS permission, and the app never crashes or gets stuck waiting on a permission that wasn't granted.

**Where I demonstrated strength**
I'm most satisfied with the database layer (`DatabaseHelper`). It cleanly separates user-account data from inventory data, uses parameterized queries to avoid SQL injection, and exposes a clear CRUD interface to the rest of the app. That class is the foundation the whole app depends on, and building it with clear method names and documentation made every other screen easier to implement correctly.

## AI Usage Acknowledgment
Generative AI (Claude, Anthropic) was used to assist in organizing and drafting this README reflection based on a review of the submitted project code. The application design, code, and final responses reflect my own work and review.
