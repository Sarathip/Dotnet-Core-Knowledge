# Task Parallel Library Dotnet Core
 
`Parallel.ForEach อีกวิธีที่จะใช้ TPL ง่ายๆ คือเปลี่ยน foreach ให้แต่ละ loop ฝาก C# แบ่งให้แต่ละ thread ทำงานช่วยกัน`

**ตัวอย่าง** ผมมีคลาส ที่ทำการทดสอบระหว่างการใช้งาน foreach กับ Parallel.ForEach
* โดยการสร้าง int array ทั้งหมด 1 ล้านตัวทำการวน loop ไปเรื่อยๆ
* เมื่อเจอค่าที่เท่ากับ 500000 จะให้ทำการแสดงผล ได้ใช้งาน Stopwatch เพื่อทดสอบเรื่องเวลา 
* ผลลัพธ์ที่ออกมา Parallel.ForEach ใช้เวลาในการทำงานน้อยกว่ามาก


 ``` c#
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
```
