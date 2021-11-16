# Book Search Guide: Pounce/Pounce+ and UofM Campuses Databases
This guide will first layout the key points of using an advanced search to find materials within our database, and then provide a template of sample
questions and how you would translate them into search conditions.  

## Advanced Search: Overview of Search Filters  
### Common Search Categories  
The first box contains categories which narrow down the fields in which given keywords will be looked for.  
- **Any field**: The default search, this works like a broad keyword search that will look for any isntance of the keyword within any of the other categories.
- **Title**: Look for books by their title/words contained within the title.
- **Author/creator**: Look for books by the author's name.
- **Subject**: Look for books by associated subject.
- **ISBN**: Look for a book by its International Standard Book Number.
- **ISSN**: Search for a book by its International Standard Serial Number.
- **Creation Date**: Serach for books created on the given date.  

### Search Parameters  
The second box contains search parameters. Parameters will decide how keywords will be looked for within a given category.  
- **Contains**: How you mihgt normally think of a keyword search, this parameter searches for any instance of the keywords. This will be the default, which you will use most of the time.
- **Is (exact)**: This parameter will only find things that exactly match the given keywords within a selected category.
- **Starts with**: This paremeter will find anything that starts with the given keywords within a selected category.  

## Translating Questions Into Search Parameters  
Following are example templates of questions you might hear from a patron and how you should connect them to a book search.  
### "Do you have ________ by ________?"  
Here you will search using both tile and author name. Most of the time, book titles will be different from how they are normally called, so it is easier
when you aren't certain you have the exact title to use the default **contains** search parameter.  

### "Do you have a book about ________?"  
Here you will search using the Subject category.  

### "Do you have any books by ________?"  
A simple Author/creator search.  

### "I can't remember the title, but it was about ________."  
Here you might think to do a general keyword search, but it would be more helpful to serach by subject and keyword both.  

## Search Practice
Now that you've seen how the advanced search is set up and some common types of searches, try finding the books below through advanced search for Pounce/Pounce+ and UofM Campuses using different categories and parameters. When finding the books, take note of things such as how many results appear, whether the book appears on one database but not another, and how more specific keywords narrows down searches.

Searching by author J.R.R Tolkien, find the first book in the Lord of the Rings trilogy, The Fellowship of the Ring.
<style>
    .hide{
    display: none;
}
</style>

<button onclick="myFunction('next1')">Next Book</button>
<div id='next1' class="hide">
    Using a keyword search with at most two keywords, find the book "China and the West: Music, Representation, and Reception". <br />
    <button onclick="myFunction('next2')">Next Book</button>
    <div id='next2' class="hide">
        <br />Find a book related to rhetoric that was created in 1978 and has been published by the Cornell University Press.
    </div>
</div>

<script>
function myFunction(id) {
    var myDiv = document.getElementById(id);
    myDiv.classList.toggle("hide");
}
</script>
