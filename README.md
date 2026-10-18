# Elementor MCP Créateur

**Turn a client mockup into a native, editable Elementor page, from the AI assistant you already use.**

Elementor MCP Créateur is a WordPress plugin that turns your site into an MCP (Model Context Protocol) server. Connect claude.ai, ChatGPT, Claude Desktop, Claude Code or Cursor, share a mockup, ask in one sentence, and the assistant builds the page with real Elementor V4 atomic elements, on your design tokens, with a structural SEO audit on every write.

[Website](https://www.mcp-elementor.com/en) · [Docs](https://www.mcp-elementor.com/en/docs) · [Comparison with other Elementor MCP servers](https://www.mcp-elementor.com/en/compare/elementor-mcp-servers) · [Changelog](https://www.mcp-elementor.com/en/changelog) · [Version française](#en-français)

![Demo: a mockup becomes a native Elementor page through MCP tool calls](assets/demo.gif)

<sub>Real tool calls made by plugin v3.3.0 on a local test site. The chat window is a recreation (labelled on screen); the client "Café Ondine" and its mockup are fictional.</sub>

> **About this repository.** The plugin is proprietary and sold at [mcp-elementor.com](https://www.mcp-elementor.com/en). This repository holds its public documentation, the MCP registry metadata ([`server.json`](server.json)) and the issue tracker for support. It contains no plugin source code.

## What it does

- **Mockup to page.** Share a screenshot, Figma link or PDF with your assistant and ask for the page. It reads your site's design system, then writes the page with Elementor V4 atomic elements (`e-flexbox`, `e-heading`, `e-paragraph`, `e-button`, `e-image`…) through Elementor's own save routine, so every block stays editable in the editor.
- **Your design system, not invented values.** Colours, fonts, sizes and spacing come from your Elementor kit's global variables. Missing tokens are created in the kit (`upsert-design-token`) and reused everywhere.
- **Structural SEO on every write.** Each tree write comes back with an audit: a single H1, no skipped heading levels, single-child container chains ("divitis"), image alt text, JSON-LD. `audit-seo` checks the rendered HTML, landmarks included; `set-schema` adds whitelisted Schema.org JSON-LD (FAQPage, Product, LocalBusiness, Article…).
- **Fidelity you can verify.** Each section is compared back against the mockup on five checks (content, decorative media, styles and tokens, layout and spacing, responsive states), in sessions that must come back clean at least five times before the page counts as done. `audit-layout` measures alignment in a real browser.
- **Content as data.** Repeatable content (FAQ, reviews, team, posts) becomes a custom post type with ACF fields, rendered by the plugin's listing and FAQ widgets instead of hard-coded text.
- **Real forms, local assets.** Forms are real `<form>` elements with a plugin widget and stored entries. Icons come from bundled Lucide and Simple Icons libraries, remote media is imported into your media library, and animations use Elementor's native interactions or bundled GSAP presets, with no CDN.

## Connect your assistant

MCP endpoint, on your own WordPress site:

```
https://{your-site}/wp-json/elementor-mcp-createur/mcp
```

**claude.ai, the Claude mobile app, Claude Desktop, ChatGPT: OAuth 2.1 sign-in (since v3.3.0).** Add a custom connector with the endpoint URL, click *Connect*, log in to WordPress with an Editor or Administrator account and approve. No password to copy. In ChatGPT, turn on developer mode in *Apps & Connectors* and create a connector with the same URL. The site must be served over HTTPS; the plugin's *Configuration MCP* screen includes a diagnostic for this.

**Claude Code, Cursor and other file-configured clients: application password.** The plugin creates a dedicated `MCP_ELEMENTOR` user (Editor role) and shows a ready-to-paste block on *Elementor MCP → Configuration MCP*:

```json
{
  "mcpServers": {
    "elementor-mcp-createur": {
      "url": "https://{your-site}/wp-json/elementor-mcp-createur/mcp",
      "headers": { "Authorization": "Basic <base64(MCP_ELEMENTOR:application-password)>" }
    }
  }
}
```

Full guide: [Connect Claude, ChatGPT or Cursor](https://www.mcp-elementor.com/en/docs/connect-ai).

## Install

Requirements: WordPress 6.9 or later, PHP 8.0 or later, Elementor 4 (editor V4, atomic elements).

1. [Buy a license](https://www.mcp-elementor.com/en). The plugin `.zip` and your license key are shown after payment and sent by email.
2. In WordPress, go to *Plugins → Add New → Upload Plugin*, upload the `.zip`, then *Activate*.
3. Open *Elementor MCP → License*, paste your key and click *Activate / Verify*.
4. Connect your assistant (see above) and ask it to run `get-site-info` to check the connection.

Full guide: [Install Elementor MCP on WordPress](https://www.mcp-elementor.com/en/docs/install) · [Generate your first section](https://www.mcp-elementor.com/en/docs/first-generation).

## Tools

38 MCP tools, grouped by what they act on:

| Area | Tools |
|---|---|
| Site | `get-site-info`, `list-post-types`, `list-documents` |
| Design system | `get-design-tokens`, `upsert-design-token`, `list-element-types`, `list-components`, `save-component`, `set-site-parts`, `list-icons`, `get-icon`, `get-animation-capabilities`, `set-page-animations` |
| Elementor tree | `get-tree`, `save-tree`, `insert-elements`, `patch-element`, `move-element`, `delete-element`, `regenerate-css`, `audit-layout` |
| WordPress content | `create-content`, `update-content`, `delete-content`, `sideload-media`, `audit-images`, `optimize-images` |
| Forms | `upsert-form` |
| CPT & ACF | `register-cpt`, `create-acf-field-group`, `set-acf-fields` |
| SEO & performance | `set-schema`, `audit-seo`, `audit-performance` |
| Mockup fidelity | `section-inventory`, `start-fidelity-session`, `record-fidelity-check`, `close-fidelity-session` |

## Pricing

**9,99 € once.** Lifetime license for one site, unlimited generations, updates and email support included, no subscription. 14-day money-back guarantee ([refund policy](https://www.mcp-elementor.com/en/legal/refund)). Need several sites? Buy one license per site.

The generation runs in your own assistant, on your own Claude or ChatGPT plan: no AI API key and no per-generation cost on our side.

## Good to know

- **Not affiliated with Elementor.** This is an independent, third-party plugin. Elementor also publishes its own official MCP; the [comparison page](https://www.mcp-elementor.com/en/compare/free-mcp-servers) says when the free options are enough.
- **Security.** OAuth tokens are audience-restricted to the MCP endpoint, stored hashed, with PKCE S256 and rotating refresh tokens. Every tool checks WordPress capabilities on the target post. Connected apps can be revoked from the plugin's admin screen.
- **What leaves your site.** The license is validated against the mcp-elementor.com API, and each tree write (`save-tree`, `insert-elements`, `save-component`) is normalised and audited there before it is saved. No AI runs on our servers, and no write goes through without that check.
- **One license, one site.** A key binds to the first site that activates it.

## Support

- Bug or question: [open an issue](https://github.com/YounessFTN/elementor-mcp/issues/new/choose) (never paste your license key or application password).
- Email: [youness@fatine.fr](mailto:youness@fatine.fr)
- Troubleshooting: [mcp-elementor.com/en/docs/troubleshooting](https://www.mcp-elementor.com/en/docs/troubleshooting)

## En français

**Elementor MCP Créateur** est un plugin WordPress qui transforme votre site en serveur MCP. Connectez claude.ai, ChatGPT, Claude Desktop, Claude Code ou Cursor, partagez la maquette du client, demandez la page en une phrase : l'assistant la construit en éléments atomiques Elementor V4, sur les variables de votre kit, modifiable dans l'éditeur, avec un audit SEO structurel à chaque écriture.

- **Endpoint** : `https://{votre-site}/wp-json/elementor-mcp-createur/mcp`
- **Connexion** : OAuth 2.1 (claude.ai, appli mobile Claude, Claude Desktop, ChatGPT) depuis la v3.3.0, ou mot de passe d'application de l'utilisateur `MCP_ELEMENTOR` (Claude Code, Cursor…).
- **Prix** : 9,99 € une seule fois, licence à vie pour un site, générations illimitées, mises à jour et support par e-mail inclus, satisfait ou remboursé 14 jours.
- **Documentation** : [mcp-elementor.com/fr/docs](https://www.mcp-elementor.com/fr/docs) · [site](https://www.mcp-elementor.com/fr)

Plugin propriétaire : ce dépôt contient uniquement la documentation, les métadonnées du registre MCP et le suivi des demandes de support.

---

Made by [Youness Fatine](https://www.fatine.fr/). © 2026 Youness Fatine. See [LICENSE](LICENSE).
