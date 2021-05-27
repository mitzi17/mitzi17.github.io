---
layout: post
title:      "JavaScript Rails Project"
date:       2021-05-27 20:03:33 +0000
permalink:  javascript_rails_project
---

For the JavaScript Rails Project I did a simple Store Article Finder app, that as the name suggests it makes it easier to know where a particular item in a store is. The app also allows to filter items by location. One of the most challenging things about the project was understanding was the implementation of fetch requests and how it works with the backend. For my project in particular, in my createArticle method, all attributes are being POSTed as seen below:

```
static createArticle() {
        const formData = {
            name: nameField.value,
            number: numberField.value,
            price: priceField.value,
            size: sizeField.value,
            category: categoryField.value,
            location_id: locationDropdown.value,
            location: locationDropdown.selectedOptions[0].innerText
            }
            
        debugger
        const configObj = {
            method: 'POST', 
            headers: { 
                "Content-Type": "application/json", 
                Accept: "application/json"
            },
            body: JSON.stringify(formData) 
        }
    
        fetch(this.baseURL, configObj)
            .then(response => response.json())
            .then(newArticleData => {
            debugger    
                
                const newArticle = new Article({id: newArticleData.id, name: newArticleData.name, number: newArticleData.number, price: newArticleData.price, size: newArticleData.size, category: newArticleData.category, location_id: newArticleData.location_id, location: newArticleData.location.area})
                newArticle.renderArticle()
                newArticleForm.reset()
            })
    }
```

One of the issues I was having was that formData was containing all attributes of an article (name, number, price, size, category, location_id, and location), however when I was fetching that recently created article, location was not being returned. I use debugger to see what was happening but no errors were being displayed. I figure since everything was being sent to backend correctly but the retrieving of data was incomplete, my fetch request was correct. The only other place I could look was the articles_controller in the backend. 

```
def create
        article = Article.new(article_params)
        
        if article.save
            render json: article.serializable_hash
						
        else
            render json: {error: "Article did not save"}
        end
    end
```

After researching on the serializable_hash, I found out that I needed to add inlcude: :location to be able to access the location instance and not just the column location_id.

```
def create
        article = Article.new(article_params)
        
        if article.save
            render json: article.serializable_hash(include: :location)
        else
            render json: {error: "Article did not save"}
        end
    end
```

By including the location, I could now have access to location in newArticleData and have it displayed on the page as soon as the user created the article.

