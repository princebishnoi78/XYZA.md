# XUVIA

A browser-based, real-time hide-and-hunt game for 3–15 players. The host can edit the starting hunter count (1–7), zone shrink time (30–300 seconds), starting zone radius (15–48% of the arena), and final radius (3–25%, smaller than the starting radius) before the round. At least one survivor is always left in the game. Tagging a survivor converts them to the hunter team. Hunters get survivor locations after 60 seconds. Survivors win if anyone is still untagged when the five-minute timer ends. Hunters win if they tag everyone first.

The safe zone shrinks over the configured duration and then stays at its final radius. The round timer remains five minutes. Players are kept inside the zone as it closes.

Player positions use the in-game arena map only. XUVIA does not request device GPS permission or use real-world coordinates.

## Run locally

Requires Python 3.9 or newer; no third-party packages are needed.

```sh
cd hidden-sick
python3 game_server.py
```

Open [http://localhost:8000](http://localhost:8000) in a browser. Create a room, share its five-character code, and invite 2–14 more players. The host can start once at least 3 players are in the lobby.

## Play on a local network

Run the server on a computer that is reachable by the other players. Find that computer's local IP address, then have everyone open `http://<server-ip>:8000`. Only share the room code with players on that server. For play over the public internet, deploy the server on a host that supports Python and make its port reachable; rooms currently live in server memory and reset when the server restarts.

## Deploy online with Render

The included `render.yaml` configures a Python web service and its health check. To deploy under your own account:

1. Push this project to a GitHub repository you control.
2. In Render, choose **New → Blueprint**, connect that repository, and deploy the `render.yaml` Blueprint.
3. Open the Render URL and share it with players.

The free service may sleep when idle, and restarting or redeploying clears active rooms because rooms are stored in server memory. Keep one server instance for room consistency. The health endpoint is `/healthz`.

## Controls

- Move: WASD or arrow keys; on a phone, use the on-screen arrows.
- Hunter tag: E or the **Tag nearby** button. Hunters must get within 4.5 arena units of a survivor.
- The game runs for five minutes. In the lobby, the host edits and saves starting hunters, zone shrink time, and starting/final zone radius before starting the game. The maximum hunter count is always one fewer than the players in the room. Tagged survivors switch teams.

The server is authoritative for room membership, roles, game settings, tags, round timing, and movement boundaries. It uses standard-library HTTP polling for live state updates.
