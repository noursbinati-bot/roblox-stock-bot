import axios from "axios";

const WEBHOOK_URL = "https://discord.com/api/webhooks/1520407354645024889/GEFDiIuBafxE5DULpy0P-u3oB_ZeBpSFQ8uVPd-k_5u4XoPWlzyt6cUQGF-8_qIgq_ih";

// ---- ITEMS (from your Roblox modules) ----
const gadgets = [
  { name: "WateringCan", display: "Watering Can", rarity: "Common" },
  { name: "LegendaryFertilizer", display: "Legendary Fertilizer", rarity: "Legendary" },
  { name: "BoostPotion", display: "Boost Potion", rarity: "Epic" },
  { name: "SpeedPotion", display: "Speed Potion", rarity: "Rare" },
  { name: "BoostResearchPotion", display: "Research Potion", rarity: "Epic" },
];

const sprinklers = [
  { name: "LightSprinkler2", display: "Light Sprinkler", rarity: "Uncommon" },
  { name: "TurboSprinkler2", display: "Turbo Sprinkler", rarity: "Rare" },
  { name: "HeavySprinkler", display: "Heavy Sprinkler", rarity: "Epic" },
];

const tools = [
  { name: "PlantPincers", display: "Plant Pincers", rarity: "Uncommon" },
  { name: "PlantMover", display: "Plant Mover", rarity: "Common" },
  { name: "ResearchStationTp", display: "Research TP", rarity: "Uncommon" },
  { name: "UpgradeStandTp", display: "Upgrade TP", rarity: "Uncommon" },
];

// ---- STOCK GENERATOR (same seed idea as Roblox) ----
function getStock() {
  const seed = Math.floor(Date.now() / 300000); // 5 min rotation

  function rng(i) {
    return Math.abs(Math.sin(seed * 999 + i * 1234)) % 1;
  }

  function roll(items) {
    const stock = {};
    items.forEach((item, i) => {
      stock[item.display] = rng(i) > 0.3 ? Math.floor(rng(i + 1) * 5) + 1 : 0;
    });
    return stock;
  }

  return {
    Gadgets: roll(gadgets),
    Sprinklers: roll(sprinklers),
    Tools: roll(tools)
  };
}

// ---- DISCORD ----
async function postStock() {
  const stock = getStock();

  const fields = Object.entries(stock).map(([cat, items]) => ({
    name: cat,
    value: Object.entries(items)
      .map(([name, qty]) => `${name}: ${qty}`)
      .join("\n"),
    inline: false
  }));

  await axios.post(WEBHOOK_URL, {
    embeds: [
      {
        title: "🌱 Shop Stock Update",
        color: 3447003,
        fields,
        timestamp: new Date().toISOString()
      }
    ]
  });

  console.log("Posted stock:", new Date().toISOString());
}

// run immediately + every 5 min
postStock();
setInterval(postStock, 300000);
