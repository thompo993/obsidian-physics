if (!ea.verifyMinimumPluginVersion || !ea.verifyMinimumPluginVersion("1.5.21")) {
  new Notice("This script requires a newer version of Excalidraw. Please update.");
  return;
}

const api = ea.getExcalidrawAPI();
let appState = api.getAppState();
appState.currentItemStrokeColor = "#1e1e1e"; // Excalidraw's default black swatch
api.updateScene({ appState, commitToHistory: false });
new Notice("Pen color: Black");