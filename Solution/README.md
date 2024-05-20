# Blazor Puzzle #34

## Static Search

YouTube Video: https://youtu.be/Qasm1K1_b4s

Blazor Puzzle Home Page: https://blazorpuzzle.com

### The Challenge:

Finish the search box...  In this puzzle, we have a search box at the top of the screen in a Blazor SSR application.  We want to connect it to our search page.

How can we do this?

The only wrong answer is one that doesn't work.

### The Solution:

We're going to use a standard HTTP Form to access the search page.

Change *MainLayout.razor* to the following:

```html
@inherits LayoutComponentBase

<div class="page">
    <div class="sidebar">
        <NavMenu />
    </div>

    <main>
        <div class="top-row px-4">
            <form class="form-inline" action="/search" method="GET">
                <input name="t" type="text" placeholder="Search..." />
            </form>
            <a href="https://learn.microsoft.com/aspnet/core/" target="_blank">About</a>
        </div>

        <article class="content px-4">
            @Body
        </article>
    </main>
</div>
```

We are using a GET method so the search parameter will be passed on the URL. That let's you bookmark it.

Change *SearchResults.razor* to the following:

```c#
@page "/search"
@inject Repository Repository

<h1>Search</h1>
<h3>Returning results for @SearchTerm</h3>

@* Output a list of the episodes *@
@if (Episodes != null)
{
	<ul>
		@foreach (var episode in Episodes)
		{
			<li>@episode.Title</li>
		}
	</ul>
}
else
{
	<p>No results found</p>
}

@code {

	[Parameter, SupplyParameterFromQuery(Name = "t")]
	public string SearchTerm { get; set; }

	public IEnumerable<Episode> Episodes { get; set; }

	protected override async Task OnParametersSetAsync()
	{

		Episodes = await Repository.Search(SearchTerm);

		await base.OnParametersSetAsync();

	}

}
```

We modified the Parameter attribute to get the value from the form query string.

Boom!
