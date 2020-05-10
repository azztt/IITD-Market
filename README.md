# IITD-Market
Proposal for a marketplace which can be used by IITD students to buy/sell items.

## Proposed features - v1.0
- All updates to be complete overwriting in stage 1 later to be done atomically
- **Login using kerberos oauth**, to allow only IITD students to access the portal.
- The landing page will consist of 2 browsing sections - **browse latest deals**, and **browse by categories**. The categories will be pre-defined, including cycles, mattresses, electronics, books, etc. 
- The user can choose to view the products according to any of the above options (latest items, or items of a particular category). The item listing page will consist of the items sorted according to the time of upload. The user can choose to sort the items in another way (e.g. sorting by price)
- Each item will have a page listing the following details: Name, description, pictures (at least one), category, condition, price, and when the seller had bought the item (month/year, month being optional). It will also provide the user an option to chat with the seller (described below)
- To sell an item, a user can post an advertisement, providing the details mentioned above. **The seller can choose to remain anonymous**.
- A potential buyer will be able to **chat with the seller** through an option provided on the item page. We propose that the buyer and seller chat through this chat window (unique to them) to clarify things about the item, arrive at a price, meeting location, etc. If a seller had chosen to remain anonymous, he/she can reveal their identity through this window.
- The seller can choose to **mark the item as sold in the chat window with a particular buyer**. This option will also be given on the item page outside the chat window, where the seller can manually enter the buyer's information (in case the deal has been made through means other than the in-portal chat). Doing this will confirm the deal from the seller's end, and disable chats with any other existing/new potential buyers.
- Once the deal has been done in the real world, the buyer is expected to return to the portal to **mark the item as received, and rate the seller.** This closes the deal from both ends, and **the item is now officially marked as sold** and shifted to the list of bought items.
- The rating provided by the buyer under ___How satisfied are you?___ at time of purchase will be added to the seller's reviews and update his/her average rating. This will get displayed on later products of the seller under Previous Customer Experience.
- Guest users will be allowed to browse the items on the portal, but not be allowed to post an advertisement, or initiate chat with a seller.
- A search bar
- And of course, an admin page to view reported stuff and perform removals/bans.

## Proposed features - Into the future
- A 'wishlist' to mark the categories the user is interested in buying. Once an item of the category gets uploaded, the user will receive a notification.
- Adding a lost and found section
- Using automod for admin review
- Giving seller an option to upload pics (only url in stage 1)
- an option for Rent?

## Implementation details
View the [Wiki](https://github.com/azztt/IITD-Market/wiki) for backend implementation details, workflow and sample designs.
