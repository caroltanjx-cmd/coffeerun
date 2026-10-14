# Coffee Run

Helps one person take drink orders for a group and relay them to a Singapore kopitiam drinks stall. For each drink, the person taking orders types who it's for and builds the drink in kopitiam terms (Kopi C Siew Dai Peng and so on). The page combines identical drinks into one tah pau list, which they can show full-screen to the uncle or copy into a chat with the stall. "Who ordered what" lists each person's drinks, for collecting money afterwards.

- GitHub Pages: https://caroltanjx-cmd.github.io/coffeerun/
- claude.ai: https://claude.ai/artifact/UwdGhda76JmeXYxjy3kTyh

## How it runs

`index.html` is a single page with no build step and no server. It works anywhere a browser can open it.

The order is saved in that browser's `localStorage` under `coffeerun.order`, so it survives a reload on the same device. It isn't shared: other people and other devices don't see it. **Start a new run** clears it.

Saved drinks are read back as untrusted: unknown drinks are dropped, options are forced into a valid shape for the drink, names are cut to 30 characters and notes to 40. An order holds at most 60 drinks.

## Drink vocabulary

The `BASES` array at the top of the script lists the drinks and which options apply to each. A drink is said in this order: base, milk, strength, sugar, iced.

| Term | Meaning |
| --- | --- |
| (none) | With condensed milk |
| C | With evaporated milk |
| O | No milk |
| Gao / Po | Strong / weak |
| Siew Dai / Kosong / Ga Dai | Less sugar / no sugar / more sugar |
| Peng | Iced |
| Tah pau | Takeaway |

Kosong is turned off for drinks with condensed milk, because condensed milk is already sweet.
