# Remap a Key to Enter Using keyd (Linux)

## Problem

After following all steps, the key remapping was not working.

> **Note:** Restart is usually not needed. Most likely one of these happened:
> - Config file not saved correctly
> - Wrong key name
> - keyd not reading config

---

## Step 1: Check if keyd is Running

```bash
sudo systemctl status keyd
```

You should see:

```
active (running)
```

---

## Step 2: Check Your Config

```bash
cat /etc/keyd/default.conf
```

It should **exactly** show:

```
[ids]

*

[main]

menu = enter
```

If not, rewrite it:

```bash
sudo nano /etc/keyd/default.conf
```

Then restart:

```bash
sudo systemctl restart keyd
```

---

## Step 3: Find the Correct Key Name

Some keyboards do **not** expose the menu key as `menu`.  
Use the monitor command to find the correct key name:

```bash
sudo keyd monitor
```

Press the key you want to use. Example output:

```
menu down
menu up
```

Or it may show:

```
compose down
```

Or:

```
rightmeta down
```

Whatever name appears — use that name in your config.

---

## Step 4: Update Config with Correct Key Name

Example using `rightmeta`:

```bash
sudo nano /etc/keyd/default.conf
```

Paste:

```
[ids]

*

[main]

rightmeta = enter
```

Then restart:

```bash
sudo systemctl restart keyd
```

---

## Real Example: Surface Keyboard Output

Running `sudo keyd monitor` showed:

```
keyd virtual keyboard    0fac:0ade:bea394c0    compose down
keyd virtual keyboard    0fac:0ade:bea394c0    compose up
```

**Key name confirmed: `compose`**

---

## Step 5: Apply the Correct Config

```bash
sudo nano /etc/keyd/default.conf
```

Paste:

```
[ids]

*

[main]

compose = enter
```

Save:
- `Ctrl + O`
- `Enter`
- `Ctrl + X`

Restart keyd:

```bash
sudo systemctl restart keyd
```

---

## Result

The `compose` (Menu) key now behaves exactly like `Enter` everywhere:

- Browser search
- VSCode
- Terminal
- Forms

---

## Is This Permanent?

**Yes. It is permanent.**

Because you already ran:

```bash
sudo systemctl enable keyd --now
```

`enable` makes keyd start automatically on every boot.

| Scenario | Behavior |
|---|---|
| After restart | keyd starts automatically |
| After shutdown | keyd starts automatically |
| compose key | Always acts as Enter |
| Run commands again? | Not needed |

> Only if you **uninstall keyd** or **change the config** will it stop working.
