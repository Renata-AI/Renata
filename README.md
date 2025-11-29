# project veridion
desc:
Logos are instrumental for a company’s identity – they’re the symbol that customers use to recognize your brand. Ideally, you’ll want people to instantly connect the sight of your logo with the memory of what your company does – and, more importantly, how it makes them feel.

The first challenge in the project was getting the actual logos. Ideally, I would extract them directly from the <head> section of each website, but a lot of sites don’t keep their logo there. Because of that, using Clearbit’s API ended up being the fastest and most reliable way to collect them across all domains.

The next problem was preprocessing. Since many logos look extremely similar and only differ in quality or small details, I found that shrinking them to a low resolution (32x32) works best. Turning them into grayscale also helps keep things both simple and accurate without wasting processing time.

The biggest challenge, though, was clustering. I tested several algorithms: DBSCAN gave too many false positives, and K-Means wasn’t practical because it needs to know the number of clusters in advance—which I didn’t. In the end, HDBSCAN worked the best. It doesn’t require the number of clusters, and because it uses density, it fits really well with how logos naturally group together. For the logos that HDBSCAN initially marks as noise, I run a second pass using SSIM with a threshold of 0.8 (chosen after testing). This lets me place most of them correctly.

Instead of showing the results in a messy CSV, I chose to generate a simple HTML file for reviewing the clusters. There’s no styling—just cluster numbers and the images under them. It’s quick to generate and makes checking the results way easier.

To keep things clean, I only included the main script (main.py) and the Parquet file from Veridion’s site. Running the script triggers the entire workflow automatically and saves the HTML preview into /cluster_reports.

I tried to keep everything as general as possible instead of hardcoding values so the script can be reused on any other list of websites. Even though some logos are basically identical and a few mistakes still happen, the accuracy is around 90–95%. And since the output is in HTML, it’s really easy to go through and manually confirm the clusters.
