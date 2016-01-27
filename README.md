# ObjectsDifference
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace ILLYDevTask
{
    class ClassA
    {
        public string name { get; set; }
        public int salary { get; set; }
        public DateTime dateOfBirth { get; set; }
        public List<Department> departments { get; private set; }  
      
        public ClassA(string Name, int Salary, DateTime DateOfBirth, List<Department> Departments)
        {
            name = Name;
            salary = Salary;
            dateOfBirth = DateOfBirth;
            departments = Departments;
        }
    }
    public class Department:IEquatable<Department>
    {
        public string Department_Name { get; set; }
        public string Country { get; set; }

        public Department(string departmentName, string country)
        {
            Department_Name = departmentName;
            Country = country;
        }
        }
   public class PropertyCompareResult
    {
        public string Name { get; private set; }
        public object OldValue { get; private set; }
        public object NewValue { get; private set; }

        public PropertyCompareResult(string name, object oldValue, object newValue)
        {
            Name = name;
            OldValue = oldValue;
            NewValue = newValue;
        }
    }
}

using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Reflection;
using System.Collections;

namespace ILLYDevTask
{
    public class Program
    {
        private static void Main(string[] args)
        {
          
            ClassA classA = new ClassA("Crage", 2500, new DateTime(2001, 10, 20),new List<Department>{new Department("HR","UK")});
            ClassA classA1 = new ClassA("Martin", 2501, new DateTime(2001, 10, 20), new List<Department>{new Department("Audit","USA")});

            List<PropertyCompareResult> resultItems = Compare(classA, classA1);
            foreach (PropertyCompareResult resultItem in resultItems)
            {
                Console.WriteLine(
                "  Property name: {0} -- old: {1}, new: {2}", resultItem.Name, resultItem.NewValue, resultItem.OldValue);
            }
           
        }


        public static List<PropertyCompareResult> Compare<T>(T oldObject, T newObject)
        {
            PropertyInfo[] properties = typeof(T).GetProperties();
            List<PropertyCompareResult> results = new List<PropertyCompareResult>();

            foreach (PropertyInfo propertyInfo in properties)
            {
                object oldValue = propertyInfo.GetValue(oldObject), newValue = propertyInfo.GetValue(newObject);
                 if (oldValue.GetType().IsGenericType)
                 {
                     Type oldproperty = oldValue.GetType().GetGenericArguments()[0];
                     PropertyInfo[] property = oldproperty.GetProperties();
                     //foreach (PropertyInfo info in property)
                     //{
                      //   object oValue = info.GetValue(oldValue);
                    // }

                 }
                if (!object.Equals(oldValue, newValue))
                {
                    results.Add(new PropertyCompareResult(propertyInfo.Name, oldValue, newValue));
                }
            }
            return results;
        }
    }
}
