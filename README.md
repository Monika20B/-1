# -1
namespace Intermediate002
{
    public class Car
    {
        string[] owner = new string[2];
        private string make;
        private int weight;
        private int horsepower;
        public double acceleration1;
        public double acceleration2;
        public int points;




        private string InputHandle(ref string input)
        {
            string output = "";
            char[] raw = input.ToCharArray();
            int i;
            for (i = 0; !('-' == raw[i] || '>' == raw[i + 1]); ++i)
            {
                output += raw[i];
            }

            input = "";
            for (int j = i + 2; j < raw.Length; j++)
            {
                input += raw[j];
            }
            return output;

        }
        private double Acc(int to, bool check)
        {
            double coef = check ? (6 + to) / 10.0 : 1;
            return (1000.0 / (2.0 / to) / (double) horsepower) * coef;
        }

        public Car(string input)
        {
            try
            {
                owner[0] = InputHandle(ref input);
                owner[1] = InputHandle(ref input);
                make = InputHandle(ref input);
                weight = Int32.Parse(InputHandle(ref input));
                horsepower = Convert.ToInt32(InputHandle(ref input));
                var a = input.ToCharArray();
                bool check;
                try
                {
                    check = a[a.Length - 3] == '1';
                }
                catch (Exception)
                { Console.WriteLine("False"); check = false; }

                acceleration1 = Acc(1, check);
                acceleration2 = Acc(2, !check);
                points = 0;

            }
            catch (Exception e)
            {
                Console.WriteLine("Exception", e);
                throw new ArgumentException(".........");
            }
        }
        public override string ToString()
        {
            return owner[0] + " " + points;
        }
    }
    class Program
    {
        static void Main(string[] args)
        {
            string command = "";
            ArrayList list = new ArrayList();

            do
            {
                Console.WriteLine("Input:");
                command = Console.ReadLine();
                if (command.ToLower().Equals("end") || command.Equals(""))
                {
                    break;
                }
                Car c = new Car(command);
                list.Add(c);
            } while (true);

            for(int i = 0; i < list.Count; ++i)
            {
                for(int j = i + 1; j < list.Count; j++)
                {
                    Race1((Car)list[i], (Car)list[j]);
                    Race2((Car)list[i], (Car)list[j]);
                }
            }

            
            Car[] arr = sort(list);
            foreach (var v in arr)
            {
                Console.WriteLine(v.ToString());
            }
        }
        static void Race1(Car a, Car b)
        {
            if(a.acceleration1 < b.acceleration2)
            {
                a.points += 3;
            }
            else
            {
                b.points += 3;
            }
        }

        static void Race2(Car a, Car b)
        {
            if(a.acceleration1 + a.acceleration2 < b.acceleration1 + b.acceleration2)
            {
                a.points += 3;
            }
            else
            {
                b.points += 3;
            }
        }
        static Car[] sort(ArrayList list)
        {
            int max = -1;
            Car[] arr = new Car[list.Count];
            for (int i = 0; i < arr.Length; ++i)
            {
                int rm = 0;
                for(int j = 0; j < list.Count; ++j) 
                {
                    if (((Car)list[j]).points > max)
                    {
                        max = ((Car)list[j]).points;
                        rm = j;
                    }
                }
                arr[i] = (Car)list[rm];
                list.RemoveAt(rm);

            }
            return arr;
        }
    }
}
