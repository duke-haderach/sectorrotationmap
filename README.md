Weekly bars — 1 data point per week instead of daily, so 8W tail = 8 dots. Clean arcs, no scribble
Data source — Yahoo Finance directly via browser fetch (no API key, no Polygon rate limits)
Tail options — 1W, 4W, 8W, 12W, 16W, 26W
Add sector — click + Add in the sidebar, type any ticker + label, it fetches and plots immediately
Remove sector — hover any row in the sidebar, click the x that appears
Click to hide/show — same as before, click the row to toggle visibility
Benchmark dropdown — header now has SPY / QQQ / DIA / IWM / VTI. Switching any of them reloads all sectors against the new benchmark and redraws automatically.
CORS fix — the fetch now tries both query1 and query2 Yahoo endpoints with the corsDomain parameter which is what Yahoo's own frontend sends. 

If your browser was blocking query1, query2 should go through. If you still get the error, open the browser console (F12) and paste the exact error message — it'll tell us whether it's a CORS block, 
a 401, or a network issue, and we can route around it.
Tries three public CORS proxies in order:

corsproxy.io — most reliable
allorigins.win — good fallback
thingproxy.freeboard.io — last resort

If the first works, it uses it for all subsequent fetches. If your corporate network blocks any of them it moves on automatically.
One caveat — these are free public proxies, fine for personal EOD use but don't abuse them. If all three fail (unlikely but possible on a locked-down network), 
the permanent solution would be a tiny local Python proxy server.
