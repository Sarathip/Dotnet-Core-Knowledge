  public JsonResult ParallelTest()
        {
            int[] arr = Enumerable.Range(0, 1000000).ToArray();
            List<string> response = new List<string>();

            Stopwatch stopWatch = new Stopwatch();
            stopWatch.Start();
            Parallel.ForEach(arr, (a) =>
            {
                if (a == 500000)
                {
                    stopWatch.Stop();
                    var elapsedTime = new TimeSpan(stopWatch.ElapsedTicks);
                    response.Add("Parallel Foreach" + elapsedTime);
                }
            });

            Stopwatch stopWatch1 = new Stopwatch();
            stopWatch1.Start();
            foreach (var a in arr)
            {
                if (a == 500000)
                {
                    stopWatch1.Stop();
                    var elapsedTime = new TimeSpan(stopWatch1.ElapsedTicks);
                    response.Add("Foreach" + elapsedTime);
                    break;
                }
            }
            return Json(new { response = response });
        }
