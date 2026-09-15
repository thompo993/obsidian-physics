if (!ea.verifyMinimumPluginVersion || !ea.verifyMinimumPluginVersion("1.5.21")) {
  new Notice("This script requires a newer version of Excalidraw. Please update.");
  return;
}

// Tweak these two numbers to taste.
const BOLD_WIDTH = 1;     // stroke width while "bold mode" is ON
const NORMAL_WIDTH = 0.5;   // fallback width if nothing was saved yet

let settings = ea.getScriptSettings();
if (!settings || settings["Bold active"] === undefined) {
  settings = {
    "Bold active": { value: false, hidden: true },
    "Saved width": { value: NORMAL_WIDTH, hidden: true }
  };
}

const api = ea.getExcalidrawAPI();
let appState = api.getAppState();
const isBold = settings["Bold active"].value;

if (!isBold) {
  // Turning bold ON: remember current width, then bump it up
  settings["Saved width"].value = appState.currentItemStrokeWidth ?? NORMAL_WIDTH;
  appState.currentItemStrokeWidth = BOLD_WIDTH;
  settings["Bold active"].value = true;
  new Notice("Bold pen: ON");
} else {
  // Turning bold OFF: restore the width it was before
  appState.currentItemStrokeWidth = settings["Saved width"].value;
  settings["Bold active"].value = false;
  new Notice("Bold pen: OFF");
}

ea.setScriptSettings(settings);
api.updateScene({ appState, commitToHistory: false });