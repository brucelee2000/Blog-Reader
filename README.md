# Blog-Reader
JSON Data Obtain and Process
----------------------------
* **Step 1. Obtain JSON data**

        let urlString = "https://www.googleapis.com/blogger/v3/blogs/10861780/posts?key=AIzaSyDDpLNDdScgBf4_6WfVklvpKwyN8lkzex0"
        let url = NSURL(string: urlString)
        let session = NSURLSession.sharedSession()
        
        let task = session.dataTaskWithURL(url!, completionHandler: { (webData, webResponse, webError) -> Void in
            if webError != nil {
                println(webError)
            } else {
            // JSON data processing
            // Check codes in step 2
            }
        })
        
        task.resume()
        
* **Step 2. Process JSON data**

  *NSJSONSerialization* class to convert JSON to Foundation objects and convert Foundation objects to JSON

        // Return the whole page as Dictionary
        let jsonResult = NSJSONSerialization.JSONObjectWithData(webData, options: NSJSONReadingOptions.MutableContainers, error: nil) as NSDictionary
                
        // Prepare space for saving data
        var myResult:[[String:String]] = []
                
        // Extract target information from JSON data

        // The value of keyword "items" is an array of dictionary which contains all the blogs
        let blogArray:[NSDictionary] = jsonResult["items"] as [NSDictionary]
        // Pickup each element in the array
        for var index = 0; index < blogArray.count; index++ {
            // Create an emmpty element
            myResult.append([String:String]())
            var myItem = myResult[index]
            var blogItem = blogArray[index]
                    
            myItem["content"] = blogItem["content"] as NSString
            myItem["title"] = blogItem["title"] as NSString
            myItem["publishedDate"] = blogItem["published"] as NSString
                    
            // The value of keyword "author" is a Dictionary
            var authorDictionary = blogItem["author"] as NSDictionary
            myItem["author"] = authorDictionary["displayName"] as NSString
        }
