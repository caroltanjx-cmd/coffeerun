# Coffee Run

Pools a group's drink orders into one list for a Singapore kopitiam drinks stall. Each person builds their drinks in kopitiam terms (Kopi C Siew Dai Peng and so on). Everyone sees the combined order live, and the runner can show it full-screen to the uncle or copy it into a chat with the stall.

Live app: https://claude.ai/artifact/UwdGhda76JmeXYxjy3kTyh

## How it runs

`index.html` is a single page with no build step. It runs as a **claude.ai artifact**. Login and storage come from the artifact runtime (`window.claude`):

- `user`: identifies the signed-in viewer, with their name and avatar.
- `db`: a shared document store.
  - `orders/<userId>` holds `{ drinks: [...], at }`, up to 3 drinks per person. Removing your last drink deletes the document.
  - `run/state` holds `{ closed, at }`. Only the owner writes it.

The page declares these capabilities when it is published:

```json
{
  "db": { "rules": [
    { "path": "orders", "read": "view", "write": "owner" },
    { "path": "orders/{self}", "write": "interact" },
    { "path": "run", "read": "view", "write": "owner" }
  ] },
  "user": { "scopes": ["profile"] }
}
```

Everyone reads every order. Each person writes only their own. Only the owner can close orders or start a new run, which deletes everyone's orders. People who order need **Contributor** access in the artifact's Share menu.

Closing orders is enforced only in the page; the db rules can't check it. The page also treats every order it reads as untrusted: unknown drinks are dropped, options are forced into a valid shape for the drink, and notes are cut to 40 characters.

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
