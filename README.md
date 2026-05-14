<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, interactive-widget=resizes-visual" />
    <title>Birthday Quest</title>
    <link rel="icon" type="image/svg+xml" href="/favicon.svg" />
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
  </head>
  <body>
    <div id="root"></div>
    <script type="module" src="/src/main.tsx"></script>
  </body>
</html>
export type Item = { id: string; name: string; emoji: string };
export type DialogLine = {
  speaker: string; text: string;
  portrait: "piotrek" | "kostka" | "forester" | "none";
};
export type RiddleConfig = {
  question: string; answer: string;
  onSuccess: () => void; onFail: () => void;
};
export type GameEvent =
  | { type: "DIALOG_OPEN"; lines: DialogLine[]; riddle?: RiddleConfig; onComplete?: () => void }
  | { type: "DIALOG_CLOSE" }
  | { type: "HINT_SHOW"; text: string }
  | { type: "HINT_HIDE" }
  | { type: "INVENTORY_UPDATE"; items: Item[] }
  | { type: "SCENE_BIRTHDAY" }
  | { type: "GAME_COMPLETE"; result: "exit" | "continue" };

class Store {
  private ls = new Set<(e: GameEvent) => void>();
  emit(e: GameEvent) { this.ls.forEach((l) => l(e)); }
  on(l: (e: GameEvent) => void) { this.ls.add(l); return () => this.ls.delete(l); }
}
export const store = new Store();
