## Beginner's Guide to Unity3D
### Coding Conventions
 1. Follow the general naming conventions accepted by the community. Here is a good example of one.
    * [C# Coding Standards and Naming Conventions](https://github.com/ktaranov/naming-convention/blob/master/C%23%20Coding%20Standards%20and%20Naming%20Conventions.md)
2. Give your variables, methods and classes a proper, self explanatory names about their duty.This is covered in the naming conventions but i have to emphasize it again. it's very important for readability, reusiblity and common sense.
    *  NOPE! 
        ```csharp
            int d;
            // elapsed time in days
            int ds;
            int dsm;
            int faid;
        ```
3. Don't use magic numbers. Nobody knows what they are, why they are there. They simple are magical. Replace them with variables or constants if necessary.
    *  Look at this hideous code.Look!! What is type 0,1,2,3,4? Ever heard of **enums**? What is '*disc*'? I don't want to solve any equations when I am coding. I am procrastinating guy. My own equations are more than enough.    
        ```csharp
            public class Class1
            {
              public decimal Calculate(decimal amount, int type, int years)
              {
                decimal result = 0;
                decimal disc = (years > 5) ? (decimal)5/100 : (decimal)years/100; 
                if (type == 1)
                {
                  result = amount;
                }
                else if (type == 2)
                {
                  result = (amount - (0.1m * amount)) - disc * (amount - (0.1m * amount));
                }
                else if (type == 3)
                {
                  result = (0.7m * amount) - disc * (0.7m * amount);
                }
                else if (type == 4)
                {
                  result = (amount - (0.5m * amount)) - disc * (amount - (0.5m * amount));
                }
                return result;
              }
            }
        ```
    * Instead, look at this relatively better code. Clear naming tells you every thing you need, no need to solve equations and puzzles. And instead of magical type 1,2,3, there is enum values.
        ```csharp
        public enum AccountStatus
        {
          NotRegistered = 1,
          SimpleCustomer = 2,
          ValuableCustomer = 3,
          MostValuableCustomer = 4
        }
        ```
        ```csharp
       public class DiscountManager
        {
          public decimal ApplyDiscount(decimal price, AccountStatus accountStatus, int timeOfHavingAccountInYears)
          {
            decimal priceAfterDiscount = 0;
            decimal discountForLoyaltyInPercentage = (timeOfHavingAccountInYears > 5) ? (decimal)5/100 : (decimal)timeOfHavingAccountInYears/100;
         
            if (accountStatus == AccountStatus.NotRegistered)
            {
              priceAfterDiscount = price;
            }
            else if (accountStatus == AccountStatus.SimpleCustomer)
            {
              priceAfterDiscount = (price - (0.1m * price)) - (discountForLoyaltyInPercentage * (price - (0.1m * price)));
            }
            else if (accountStatus == AccountStatus.ValuableCustomer)
            {
              priceAfterDiscount = (0.7m * price) - (discountForLoyaltyInPercentage * (0.7m * price));
            }
            else if (accountStatus == AccountStatus.MostValuableCustomer)
            {
              priceAfterDiscount = (price - (0.5m * price)) - (discountForLoyaltyInPercentage * (price - (0.5m * price)));
            }
            return priceAfterDiscount;
          }
        }
         ```
        Further reading: [how-to-make-a-good-code](https://www.codeproject.com/Articles/1083348/Csharp-BAD-PRACTICES-Learn-how-to-make-a-good-code). 
4. Try to stick with object oriented programming (OOP) principles if you are not using other architectures deliberately. 
    * Create classes for fundemantal or meta/data objects if necessary.
    * It is good practice that every class has a single job to handle. Don't overuse a single class for multiple tasks.
    * Name every class according to its task. Don't write player, Move etc. [It's outrageous, egregious, preposterous.](https://www.youtube.com/watch?v=X8rxPrV-tn4).
    These are not OOP principles by the way. Here is some readings about basic OOP concepts.
    - [introduction-to-object-oriented-programming-concepts-in-C-Sharp.](https://www.c-sharpcorner.com/UploadFile/mkagrahari/introduction-to-object-oriented-programming-concepts-in-C-Sharp/)
    -  [oop-concepts-c-sharp](https://stackify.com/oop-concepts-c-sharp/).
    -  [object-oriented-programming](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/object-oriented-programming).
5. You can use StringBuilder class for string comparisons instead of hard coded string comparisons.
6. By the don't hard-code strings. Create variables, constants or something. :)
### Unity Tips
1. You can use structs for small data objects.
2. ScriptableObjects  are awesome way to store data such as achivements, your levels depends to the game, player info etc. They are independent of classes an gameobjects so you can use it anywhere and not lose it.
3. Keep global variables as low as possible. Use [SerializeField] attribute to access it in the Editor and [HideInInspector] to hide your global variables.
4. Keep your **Assets** folder in order. Don't just import every model, sprite in main Assets folder. [It's outrageous, egregious, preposterous.](https://www.youtube.com/watch?v=X8rxPrV-tn4).
    ```
    |-- Assets
        |-- Animations
        |   |-- 2DAnimations
        |   |-- 3DAnimations
        |-- Editor           //Special Folder
        |-- Media
        |   |-- Audio
        |   |-- Video
        |-- Models
        |-- Materials
        |-- Plugins          //Special Folder
        |   |-- Android
        |   |-- iOS
        |   |-- Special
        |-- Prefabs
        |-- Resources        //Special Folder
        |   |-- Android
        |   |-- iOS
        |   |-- Common
        |-- Scenes
        |   |-- Levels
        |   |-- Others
        |-- Scripts
        |   |-- Editor
        |-- Shaders
        |-- StreamingAssets  //Special Folder
        |-- SandBox
        |-- Sprites
    ```
    This one is a good example. There are lots of ways to do this,just be organised.
5.  Use CompareTag function instead  string comparisons.
    ```csharp
        if (other.gameObject.CompareTag("Player"))
        {
            Destroy(other.gameObject);
        }
    ```
6. Learn to Profiler tool in unity. [Grapy](https://github.com/Tayx94/graphy) is good monitoring tool for your mobile games.
7. Don't ignore Collision Matrix in Unity3D. Limiting collisions in your game could be  more beneficial to performance of your game than you think.  
    ![Tux, the Linux mascot](http://www.datasavvies.com/wp-content/uploads/2019/04/Screenshot_18.png)
8. Stay away using Camera.main any where you want. Just assign it to a variable and call it in start. Unity searches all objects with tag "MainCamera" and gets the camera component of that game object.
    ```csharp
    GameObject.FindGameObjectWithTag("MainCamera").GetComponent<Camera>();
    ```
    You don't want to call this in every few lines, don't you? Don't you???
9. Use **Raycasts** with  __maxDistance__  parameter. And try to use **Layermask** for if possible. Checking every collider you hit with the ray,as you can guess, is less performant.
10. Try using **TextMeshPro** package for your text needs. You can get more crisp text and usually more performant.
11. Use **tweening** for simple animations. Not just simple animations though, you can achieve a lot of things with tweening if you have some skill, just saying. No need to mention that they are more performant and easier to use. AAand free.:) 
    * [DOTween](https://assetstore.unity.com/packages/tools/animation/dotween-hotween-v2-27676)
    * [LeanTween](https://assetstore.unity.com/packages/tools/animation/leantween-3595)
    * [iTween](https://assetstore.unity.com/packages/tools/animation/itween-84)
12. This is more about programming but still. Learn to utilize [LINQ](https://docs.microsoft.com/en-us/dotnet/standard/using-linq) library. There will be times that it can be very handy.
13. Object Pooling. just search it or you can read [this](https://www.raywenderlich.com/847-object-pooling-in-unity), you will understand its importance.
    * Here is an [example script](https://gist.github.com/quill18/5a7cfffae68892621267). 
14. You can read this gamasutra article for deeper info, a little bit old but still relavent [article](https://www.gamasutra.com/blogs/HermanTulleken/20160812/279100/50_Tips_and_Best_Practices_for_Unity_2016_Edition.php).
### Contribute
 I am no expert, just a bit experienced than some. So if you see incorret or incomplete information, simply create a pull request. 
