# Publishing Scripts

TradingView hosts a large global community of Pine Script® programmers and millions of traders. Script authors can publish their custom indicator scripts, strategies, and libraries publicly in the Community scripts repository, allowing others in our community to use and learn from them. They can also publish private scripts to create drafts for public releases, test features, or collaborate with friends.

This page explains the script publishing process and provides recommendations to help authors publish their Pine scripts effectively.

> [!IMPORTANT]
> **Notice**: Before you publish a script, ensure you read and understand TradingView's House Rules, Script Publishing Rules, and Vendor Requirements.

---

## Script Publications

When an editable script is on the chart and opened in the Pine Editor, users can select the **Publish indicator/strategy/library** button in the top-right corner to open the "Publish script" window. 

After completing the preparation steps and publishing, TradingView generates a dedicated script page. This page showcases:
*   An enlarged view of the published chart (or the Strategy Tester report for strategies).
*   The publication's complete description and release notes.
*   User comments, boosts, and sharing options.

---

## Privacy Types

Authors set a script publication's privacy type using the "Privacy settings" field in the publishing window.

> [!CAUTION]
> Ensure you select the correct privacy option during preparation, as you **cannot** change a script's privacy type after it has been published.

### 1. Public
*   Available in the global Community scripts feed and search menus.
*   Must comply strictly with House Rules, Script Publishing Rules, and Vendor Requirements. Non-compliant scripts will be hidden by moderators.
*   **15-Minute Rule**: Authors have only 15 minutes to edit or delete a public script. After this, it is finalized in the version history permanently.

### 2. Private
*   Hidden from the Community scripts feed and search features.
*   Visible only to the author (via their profile) and users with the direct URL.
*   **Always editable and deletable**. This makes private publications ideal as draft platforms for testing code before a public release.
*   *Note*: You cannot link to or reference private scripts in public TradingView content.

---

## Visibility Types

Visibility determines who can view the source code and who can execute the script.

> [!CAUTION]
> As with privacy settings, you **cannot** change a script's visibility type after publishing.

### 1. Open
*   **Open-Source**: Anyone viewing the script page or adding the script to their chart can inspect the Pine Script source code.
*   Uses the Mozilla Public License 2.0 by default (custom license headers can be specified in code comments).
*   Any script reusing code from an open-source script must follow open-source code reuse requirements.

### 2. Protected
*   **Closed-Source**: The script is executable by any user, but the source code is hidden from everyone except the author.
*   Requires a paid TradingView plan to publish.
*   Used for unique scripts where logic needs protection, not for simple copies of open-source scripts.

### 3. Invite-Only
*   **Closed-Source & Restricted**: The source code is hidden, and only users specifically invited (authorized) by the author can add the script to their charts.
*   Requires a Premium or higher plan to publish.
*   Invite-only is the **only** visibility type that allows authors to charge users for access. Vendors must comply with the Vendor Requirements.

---

## Preparing a Publication

### Source Code Checks
Before opening the publishing wizard, verify your code:
*   Ensure calculations are correct and do not produce runtime errors or excessive resource usage.
*   Remove lookahead bias in `request.security()` requests (avoid using `barmerge.lookahead_on` with non-offset series on historical bars).
*   Use `minval`, `maxval`, and `options` constraints on inputs to handle invalid inputs safely.
*   Add comments and organize structure (see the [Style Guide](style-guide.md)).

### Chart Cleanliness
The publication copies your active chart layout.
*   Remove unrelated drawings, indicators, or shapes from your chart.
*   Ensure the symbol and timeframe status lines are visible.
*   Use default inputs on the active indicator before publishing so users see the default behavior when adding it.
*   **Do not use non-standard charts** (Heikin Ashi, Renko, etc.) to publish strategies or indicators that generate trade alerts, as synthetic prices can create misleading backtest/alert results.

### Strategy Report Requirements
If publishing a strategy:
*   Use a realistic starting `initial_capital` that represents an average trader's account size.
*   Set realistic `commission_*` and `slippage` parameters.
*   Keep trading size risk settings sustainable (under 10% equity per position).
*   Ensure the default settings generate a sufficient sample size of trades (ideally 100+).

---

## Title and Description Formatting

The publication wizard uses BBCode-style tags to format the description:

| Tag Syntax | Output Effect |
| :--- | :--- |
| `[b]text[/b]` | **Bold text** |
| `[i]text[/i]` | *Italic text* |
| `[s]text[/s]` | ~~Strikethrough text~~ |
| `[pine]code[/pine]` | Syntax-highlighted Pine Code block |
| `[list] [*]item [/list]` | Bulleted list formatting |
| `[quote]text[/quote]` | Blockquote container |
| `[url=link]Label[/url]` | Hyperlink insertion |
| `[image]url[/image]` | Embeds a chart snapshot image |
| `$TICKER` (e.g. `$AMEX:SPY`) | Auto-links to the symbol's TradingView overview page |

### Description Best Practices
*   Write clear, self-contained documentation explaining what the script does, how it works, and how to use it.
*   Provide English descriptions first. If the script UI is in another language, supply translations.
*   Do not include website links or social media handles in public script text.

---

## Script Updates and Release Notes

When updating an existing script:
*   Confirm the local code differs from the last published version.
*   Open the publishing wizard and select **Update existing script**.
*   Select the target script title from the menu.
*   Optionally check **Update chart** to replace the published chart preview.
*   Write **Release Notes** explaining the changes made in the version update.

---

## Tips: Using Private Drafts

Since public descriptions are locked after 15 minutes, we recommend this process for publishing:
1. Publish your script as **Private** first.
2. Review the script page, chart preview, and text formatting.
3. If there are mistakes, edit the description or publish a private update to fix the chart/code.
4. Once verified, copy the description raw text from the private draft.
5. Create your new **Public** publication using the verified code and text.
6. Delete the private draft version.
