# Unit Testing Django and Wagtail

- [Additional reading](#additional-reading)
    - [Documentation](#documentation)
    - [Inspiration](#inspiration)
- [Setting up tests](#setting-up-tests)
    - [Chosing a `TestCase` base class](#choosing-a-testcase-base-class)
    - [Providing test data](#providing-test-data)
    - [Overriding settings](#overriding-settings)
    - [Mocking](#mocking)
- [Running tests](#running-tests)

- [Common test patterns](#common-test-patterns)
    - [Django models](#django-models)
    - [Django views](#django-views)
    - [Wagtail pages](#wagtail-pages)
    - [Wagtail blocks](#wagtail-blocks)
    - [Wagtail admin customization](#wagtail-admin-customizations)
    - [Django management commands](#django-management-commands)
    - [Feature-flagged code](#feature-flagged-code)

## Additional reading

### Documentation

- [The Django testing documentation](https://docs.djangoproject.com/en/1.11/topics/testing/overview/)
- [The Wagtail testing documentation](https://docs.wagtail.io/en/stable/advanced_topics/testing.html)
- [Real Python's "Testing in Django"](https://realpython.com/testing-in-django-part-1-best-practices-and-examples/)

### Inspiration

- [The Django tests](https://github.com/django/django/tree/master/django): it's always a good idea to take a look at how Django tests code that's similar to yours.
- [The Wagtail tests](https://github.com/wagtail/wagtail/): it's also a good idea to see how Wagtail's built-in pages, blocks, etc, are tested.

## Setting up tests

### Choosing a `TestCase` base class

When writing unit tests for code in consumerfinance.gov there are multiple possible test case base classes that can be used.

- [`unittest.TestCase`](https://docs.python.org/3.6/library/unittest.html#unittest.TestCase): For testing Python code that does not interact with Django or Wagtail. This class provides all the base Python unit test assertions, such as:

    - [`assertEqual(a, b)`](https://docs.python.org/3.6/library/unittest.html#unittest.TestCase.assertEqual)
    - [`assertNotEqual(a, b)`](https://docs.python.org/3.6/library/unittest.html#unittest.TestCase.assertNotEqual)
    - [`assertTrue(x)`](https://docs.python.org/3.6/library/unittest.html#unittest.TestCase.assertTrue)
    - [`assertFalse(x)`](https://docs.python.org/3.6/library/unittest.html#unittest.TestCase.assertFalse)
    - [`assertIs(a, b)`](https://docs.python.org/3.6/library/unittest.html#unittest.TestCase.assertIs)
    - [`assertIsNot(a, b)`](https://docs.python.org/3.6/library/unittest.html#unittest.TestCase.assertIsNot)
    - [`assertIsNone(x)`](https://docs.python.org/3.6/library/unittest.html#unittest.TestCase.assertIsNone)
    - [`assertIsNotNone(x)`](https://docs.python.org/3.6/library/unittest.html#unittest.TestCase.assertIsNotNone)
    - [`assertIn(a, b)`](https://docs.python.org/3.6/library/unittest.html#unittest.TestCase.assertIn)
    - [`assertNotIn(a, b)`](https://docs.python.org/3.6/library/unittest.html#unittest.TestCase.assertNotIn)
    - [`assertIsInstance(a, b)`](https://docs.python.org/3.6/library/unittest.html#unittest.TestCase.assertIsInstance)
    - [`assertNotIsInstance(a, b)`](https://docs.python.org/3.6/library/unittest.html#unittest.TestCase.assertNotIsInstance)

- [`django.test.SimpleTestCase`](https://docs.djangoproject.com/en/1.11/topics/testing/tools/#simpletestcase): For testing Django or Wagtail code that does not interact with the database. This class provides all the assertions that `unittest.TestCase` provides as well as:

    - [`assertRaisesMessage(expected_exception, expected_message, callable, *args, **kwargs)`](https://docs.djangoproject.com/en/1.11/topics/testing/tools/#django.test.SimpleTestCase.assertRaisesMessage)
    - [`assertFieldOutput(fieldclass, valid, invalid, field_args=None, field_kwargs=None, empty_value='')[`](https://docs.djangoproject.com/en/1.11/topics/testing/tools/#django.test.SimpleTestCase.assertFieldOutput)
    - [`assertFormError(response, form, field, errors, msg_prefix='')`](https://docs.djangoproject.com/en/1.11/topics/testing/tools/#django.test.SimpleTestCase.assertFormError)
    - [`assertFormsetError(response, formset, form_index, field, errors, msg_prefix='')`](https://docs.djangoproject.com/en/1.11/topics/testing/tools/#django.test.SimpleTestCase.assertFormsetError)
    - [`assertContains(response, text, count=None, status_code=200, msg_prefix='', html=False)`](https://docs.djangoproject.com/en/1.11/topics/testing/tools/#django.test.SimpleTestCase.assertContains)
    - [`assertNotContains(response, text, status_code=200, msg_prefix='', html=False)`](https://docs.djangoproject.com/en/1.11/topics/testing/tools/#django.test.SimpleTestCase.assertNotContains)
    - [`assertTemplateUsed(response, template_name, msg_prefix='', count=None)`](https://docs.djangoproject.com/en/1.11/topics/testing/tools/#django.test.SimpleTestCase.assertTemplateUsed)
    - [`assertTemplateNotUsed(response, template_name, msg_prefix='')`](https://docs.djangoproject.com/en/1.11/topics/testing/tools/#django.test.SimpleTestCase.assertTemplateNotUsed)
    - [`assertRedirects(response, expected_url, status_code=302, target_status_code=200, msg_prefix='', fetch_redirect_response=True)`](https://docs.djangoproject.com/en/1.11/topics/testing/tools/#django.test.SimpleTestCase.assertRedirects)
    - [`assertHTMLEqual(html1, html2, msg=None)`](https://docs.djangoproject.com/en/1.11/topics/testing/tools/#django.test.SimpleTestCase.assertHTMLEqual)
    - [`assertHTMLNotEqual(html1, html2, msg=None)`](https://docs.djangoproject.com/en/1.11/topics/testing/tools/#django.test.SimpleTestCase.assertHTMLNotEqual)
    - [`assertXMLEqual(xml1, xml2, msg=None)`](https://docs.djangoproject.com/en/1.11/topics/testing/tools/#django.test.SimpleTestCase.assertXMLEqual)
    - [`assertXMLNotEqual(xml1, xml2, msg=None)`](https://docs.djangoproject.com/en/1.11/topics/testing/tools/#django.test.SimpleTestCase.assertXMLNotEqual)
    - [`assertInHTML(needle, haystack, count=None, msg_prefix='')`](https://docs.djangoproject.com/en/1.11/topics/testing/tools/#django.test.SimpleTestCase.assertInHTML)
    - [`assertJSONEqual(raw, expected_data, msg=None)`](https://docs.djangoproject.com/en/1.11/topics/testing/tools/#django.test.SimpleTestCase.assertJSONEqual)
    - [`assertJSONNotEqual(raw, expected_data, msg=None)`](https://docs.djangoproject.com/en/1.11/topics/testing/tools/#django.test.SimpleTestCase.assertJSONNotEqual)

- [`django.test.TestCase`](https://docs.djangoproject.com/en/1.11/topics/testing/tools/#testcase): For testing Django or Wagtail code that may need access to the database. This class provides all of the asserts that `django.tests.SimpleTestCase` provides as well as:

    - [`assertQuerysetEqual(qs, values, transform=repr, ordered=True, msg=None)`](https://docs.djangoproject.com/en/1.11/topics/testing/tools/#django.test.TransactionTestCase.assertQuerysetEqual)
    - [`assertNumQueries(num, func, *args, **kwargs)[`](https://docs.djangoproject.com/en/1.11/topics/testing/tools/#django.test.TransactionTestCase.assertNumQueries)

### Providing test data

There are a few different ways to provide data for your tests to operate on.

- Using [Django test fixtures](https://docs.djangoproject.com/en/1.11/topics/testing/tools/#fixture-loading) to load specific data into the database.

    Generally we use Django test fixtures when we need to test a fairly large amount of data with fixed values that matter to multiple tests. For example, when [testing the interactive regulations search indexes](https://github.com/cfpb/consumerfinance.gov/blob/main/cfgov/regulations3k/tests/test_search_indexes.py#L16).

    ```python
    class RegulationIndexTestCase(TestCase):
        fixtures = ['tree_limb.json']
        index = RegulationParagraphIndex()

        def test_index(self):
            self.assertEqual(self.index.get_model(), SectionParagraph)
            self.assertEqual(self.index.index_queryset().count(), 13)
            self.assertIs(
                self.index.index_queryset().first().section.subpart.version.draft,
                False
            )
    ```

    The best way to create test fixtures is to add the objects manually and then use the [Django `manage.py dumpdata` command](https://docs.djangoproject.com/en/1.11/ref/django-admin/#django-admin-dumpdata) to dump the objects to JSON.

- Using [Model Bakery](https://model-bakery.readthedocs.io/en/latest/index.html) to create test data automatically in code.

    Generally use use Model Bakery when we need to test operations on a model whose values are unimportant to the outcome. Occasionlly we will pass specific values to Model Bakery when those values are important to the tests. An example is when [testing operations around image models and the way they're handled](https://github.com/cfpb/consumerfinance.gov/blob/main/cfgov/v1/tests/test_meta_image.py#L14), when we want to make sure that the model gets rendered correctly.

    ```python
    class TestMetaImage(TestCase):
        def setUp(self):
            self.preview_image = baker.prepare(CFGOVImage)
            self.social_sharing_image = baker.prepare(CFGOVImage)

        def test_meta_image_both(self):
            page = baker.prepare(
                AbstractFilterPage,
                social_sharing_image=self.social_sharing_image,
                preview_image=self.preview_image
            )
            self.assertEqual(page.meta_image, page.social_sharing_image)
    ```

- Creating specific instances of models in code.

    All other test data needs are generally accomplished by creating instances of models directly. For example, our [Django-Flags `DatabaseFlagsSource` test case](https://github.com/cfpb/django-flags/blob/main/flags/tests/test_sources.py#L46-L56) tests that a database-stored `FlagState` object is provided by the `DatabaseFlagsSource` class. To do that it creates a specific instance of `FlagState` using `FlagStage.objects.create()` that it expects to be returned:


    ```python
    class DatabaseFlagsSourceTestCase(TestCase):

        def test_get_flags(self):
            FlagState.objects.create(
                name='MY_FLAG',
                condition='boolean',
                value='False'
            )
            source = DatabaseFlagsSource()
            flags = source.get_flags()
            self.assertEqual(flags, {'MY_FLAG': [Condition('boolean', 'False'), ]})
    ```


### Overriding settings

It is sometimes useful to modify Django settings when testing code that may behave differently with different settings. To do this there are two decorators that can be applied to either an entire `TestCase` class or to individual test methods:

- [`@override_settings()`](https://docs.djangoproject.com/en/1.11/topics/testing/tools/#django.test.override_settings) will override the contents of a setting variable.

  We use `@override_settings` any time the outcome of the code being tested depends on a settings variable.

  This can include testing different values of feature flags:

  ```python
  @override_settings(FLAGS={'MYFLAG': [('boolean', True)]})
  ```

  Or when we need to test behavior with AWS S3:

  ```python
  @override_settings(AWS_STORAGE_BUCKET_NAME='test.bucket')
  ```

- [`@modify_settings()`](https://docs.djangoproject.com/en/1.11/topics/testing/tools/#django.test.modify_settings) will modify specific values within a list setting.

### Mocking

#### General Mocking with Mock

We use the [Python Mock library](https://docs.python.org/3/library/unittest.mock.html) when we need to. We recommend mocking for:

- External service calls.

    For example, we have a custom Wagtail admin view that allows users to flush the Akamai cache for specific URLs. One of the tests for that view mocks calls to the Akamai `purge()` method within the module being tested to ensure that it is called with the URL that needs to be purged:

    ```python
    from django.test import TestCase
    import mock

    class TestCDNManagementView(TestCase):

        @mock.patch('v1.models.akamai_backend.AkamaiBackend.purge')
        def test_submission_with_url(self, mock_purge):
            self.client.login(username='cdn', password='password')
            self.client.post(reverse('manage-cdn'),
                             {'url': 'http://fake.gov'})
            mock_purge.assert_called_with('http://fake.gov')
    ```

- Logging introspection, to ensure that a message that should be logged does get logged.

    For example, to mock a module-level logger initialzed with `logger = logging.getLogger(__name__)`, we can patch the the `logging.Logger.info` method and make asserts based on its call arguments:

    ```python
    @patch('logging.Logger.info')
    def test_message_gets_logged(self, mock_logger_info):
        function_that_logs()
        mock_logger_info.assert_called_with('A message that gets logged')
    ```

There are other potential uses of Mock, but generally we prefer to test our code operating on real objects as much as as possible rather than mocking.

#### Mocking Requests with Responses

For mocking HTTP calls that are made via [the Requests library](http://docs.python-requests.org/en/master/), we prefer the use of [Responses](https://github.com/getsentry/responses). For example, to test whether an informative banner is displayed to users [the ComplaintLandingView tests](https://github.com/cfpb/consumerfinance.gov/blob/main/cfgov/legacy/tests/views/test_complaint.py#L67) use responses to provide response data for calls to `requests.get()`:

```python
from django.test import TestCase

import responses

class ComplaintLandingViewTests(TestCase):

    @responses.activate
    def test_no_banner_when_data_invalid(self):
        data_json = {
            'wrong_key': 5
        }
        responses.add(responses.GET, self.test_url, json=data_json)
        response = ComplaintLandingView.as_view()(self.request)
        self.assertNotContains(response, 'show-')
```

#### Mocking boto with moto

When we need to mock AWS services that are called via [boto](https://github.com/boto/boto/) we use the [moto](https://github.com/spulec/moto) library. For example, [to test our S3 utilities](https://github.com/cfpb/consumerfinance.gov/blob/main/cfgov/v1/tests/test_s3utils.py#L21-L25), we initialize moto in our test case's `setUp` method:

```python
class S3UtilsTestCase(TestCase):
    def setUp(self):
        mock_s3 = moto.mock_s3()
        mock_s3.start()
        self.addCleanup(mock_s3.stop)
```

From there calls to boto's S3 API will use the moto mock S3.


## Running tests

To run Django and Wagtail unit tests we prefer to use [tox](https://tox.readthedocs.io/en/latest/). tox creates and manages virtual environments for running tests against multiple versions of dependencies.

CFPB has a [sample `tox.ini`](https://github.com/cfpb/development/blob/main/tox.ini) that will test against Django 1.11 and 2.1 and Python 3.6. Additionally, running the tests for [CFPB's consumerfinance.gov Django project are documented with that project](https://cfpb.github.io/consumerfinance.gov/testing-be/).


## Common test patterns

### Django models

Any custom method or properties on Django models should be unit tested.  We generally use `django.test.TestCase` as the base class because testing models is going to involve creating them in the test database.

For example, interactive regulations `Part` objects [construct a full CFR title string from their fields](https://github.com/cfpb/consumerfinance.gov/blob/main/cfgov/regulations3k/models/django.py#L58-L61):

```python
class Part(models.Model):
    cfr_title_number = models.CharField(max_length=255)
    part_number = models.CharField(max_length=255)
    letter_code = models.CharField(max_length=10)

    @property
    def cfr_title(self):
        return "{} CFR Part {} (Regulation {})".format(
            self.cfr_title_number, self.part_number, self.letter_code
        )
```

This property [can be tested](https://github.com/cfpb/consumerfinance.gov/blob/main/cfgov/regulations3k/tests/test_models.py#L196-L203) against a [Model Bakery-created model](#providing-test-data):

```python
from django.test import TestCase
from regulations3k.models.django import Part

class RegModelTests(TestCase):
    def setUp(self):
        self.part = baker.make(Part)

    def test_part_cfr_title(self):
        self.assertEqual(
            self.part.cfr_title,
            "{} CFR Part {} (Regulation {})".format(
                part.cfr_title_number,
                part.part_number,
                part.letter_code
            )
        )
```

### Django views

Testing Django views requires responding to a `request` object. Django provides multiple ways to provide such objects:

- [`django.test.Client`](https://docs.djangoproject.com/en/1.11/topics/testing/tools/#the-test-client) is a dummy web browser that can perform `GET` and `POST` requests on a URL and return the full HTTP response.

    `Client` is useful for when you need to ensure the view is called appropriately from its URL pattern and whether it returns the correct HTTP headers and status codes in its response. All `django.test.TestCase` subclasses get a `self.client` object that is ready to make requests.

    *Note*: Requests made with `django.test.Client` include all Django request handling, including middleware. See [overriding settings](#overriding-settings) if this is a problem.

    The mortgage performance tests use a [combination of fixtures and Model Bakery-created models](https://github.com/cfpb/consumerfinance.gov/blob/main/cfgov/data_research/tests/test_views.py#L47-L148) to set up for testing the timeseries view's response code and response data:

    ```python
    from django.test import TestCase
    from django.core.urlresolvers import reverse

    class TimeseriesViewTests(django.test.TestCase):
        ...
        def test_metadata_request_bad_meta_name(self):
            response = self.client.get(
                reverse(
                    'data_research_api_metadata',
                    kwargs={'meta_name': 'xxx'})
                )
            self.assertEqual(response.status_code, 200)
            self.assertIn('No metadata object found.', response.content)
    ```

    Note the use of `django.core.urlresolvers.reverse` and named URL patterns to look up the URL rather than hard-coding URLs directly in tests.

- [`django.test.RequestFactory`](https://docs.djangoproject.com/en/1.11/topics/testing/advanced/#django.test.RequestFactory) shares the `django.test.Client` API, but instead of performing the request it simply generates a `request` object that can then be passed to a view manually.

    Using a `RequestFactory` generated request is useful when you wish to call the view function or class directly, without going through Django's URL dispatcher or any middleware.

    The Data & Research [conference registration handler tests](https://github.com/cfpb/consumerfinance.gov/blob/main/cfgov/v1/tests/models/test_base.py#L21) use a `RequestFactory` to generate requests with various inputs, including how a `GET` parameter is handled:

    ```python
    from django.test import RequestFactory , TestCase

    class TestConferenceRegistrationHandler(TestCase):
        def setUp(self):
            self.factory = RequestFactory()

        def test_request_with_query_string_marks_successful_submission(self):
            request = self.factory.get('/?success')
            handler = ConferenceRegistrationHandler(
                request=request,
            )
            response = handler.process(is_submitted=False)
            self.assertTrue(response['is_successful_submission'])
    ```

We generally do not recommend creating `django.http.HttpRequest` objects directly when testing views.

### Wagtail pages

[Wagtail pages](https://docs.wagtail.io/en/stable/topics/pages.html) are special kinds of Django models that form the basis of the Content Management System. They provide many opportunities to override default methods (like `get_template()`) which need testing just like [Django models](#django-models), but they also provide their own view via the `serve()` method, which makes them testable like [Django views](#django-views).

In general the same principle applies to Wagtail pages as to Django models: any custom method or properties should be unit tested.

For example, the careers `JobListingPage` [overrides the default `get_context()` method](https://github.com/cfpb/consumerfinance.gov/blob/main/cfgov/jobmanager/models/pages.py#L140) to provide additional context when rendering the page.

```python
from django.test import TestCase, RequestFactory
from jobmanager.models.django import City, Region, State
from jobmanager.models.pages import JobListingPage

class JobListingPageTestCase(TestCase):
    def setUp(self):
        self.factory = RequestFactory()

    def test_context_for_page_with_region_location(self):
        region = baker.make(Region)
        state = baker.make(State, region=region)
        city = City(name='Townsville', state=state)
        region.cities.add(city)
        page = baker.make(JobListingPage, location=region)

        test_context = page.get_context(self.factory.get('/'))

        self.assertEqual(len(test_context['states']), 1)
        self.assertEqual(test_context['states'][0], state.abbreviation)
        self.assertEqual(len(test_context['cities']), 1)
        self.assertIn(test_context['cities'][0].name, city.name)
```

### Wagtail blocks

[Wagtail StreamFields](https://docs.wagtail.io/en/stable/topics/streamfield.html) provide blocks that can be added to Wagtail pages. Blocks define a field schema and render those fields independent of the rest of the page.

As with Wagtail pages and Django models, any custom method or properties should be unit tested. Additionally, it may be important to test to the way a block renders. For example, the basic Wagtail [`DateTimeBlock` unit test](https://github.com/wagtail/wagtail/blob/master/wagtail/core/tests/test_blocks.py#L2950-L2963) tests that the block renders the date value that's given:

```python
from datetime import datetime
from django.test import SimpleTestCase
from wagtail.core import blocks

class TestDateTimeBlock(SimpleTestCase):
    def test_render_form_with_format(self):
        block = blocks.DateTimeBlock(format='%d.%m.%Y %H:%M')
        value = datetime(2015, 8, 13, 10, 0)
        result = block.render_form(value, prefix='datetimeblock')
        self.assertIn(
            '"format": "d.m.Y H:i"',
            result
        )
        self.assertInHTML(
            '<input id="datetimeblock" name="datetimeblock" placeholder="" type="text" value="13.08.2015 10:00" autocomplete="off" />',
            result
)
```

More complicated blocks' fields are stored as JSON on the page object. To prepare that JSON as the block's `value` to pass to the block's `render()` and other methods, the `block.to_python()` method is used.

For example, the `TextIntroduction` block [requires its `heading` field when the `eyebrow` field is given](https://github.com/cfpb/consumerfinance.gov/blob/main/cfgov/v1/atomic_elements/molecules.py#L98-L105). This is tested by checking if a [`ValidationError` is raised in `block.clean(value)`](https://github.com/cfpb/consumerfinance.gov/blob/main/cfgov/v1/tests/atomic_elements/test_molecules.py#L250-L255). To prepare a value to pass, `to_python()` is used:

```python
from django.test import SimpleTestCase
from v1.atomic_elements.molecules import TextIntroduction

class TestTextIntroductionValidation(SimpleTestCase):
    def test_text_intro_with_eyebrow_but_no_heading_fails_validation(self):
        block = TextIntroduction()
        value = block.to_python({'eyebrow': 'Eyebrow'})

        with self.assertRaises(ValidationError):
            block.clean(value)
```

### Wagtail admin customizations

It is occasionally useful to test custom Wagtail admin views and `ModelAdmin`s for Django models.

When testing the admin Wagtail contains a useful mixin class for test cases, `wagtail.tests.utils.WagtailTestUtils`, which provides a `login()` method which will create a superuser and login for all `self.client` requests.

For example, this is used [when testing the Wagtail-Flags admin views](https://github.com/cfpb/wagtail-flags/blob/main/wagtailflags/tests/test_views.py#L8-L11):

```python
from django.test import TestCase
from wagtail.tests.utils import WagtailTestUtils

class TestWagtailFlagsViews(TestCase, WagtailTestUtils):
    def setUp(self):
        self.login()
```

The custom admin view that Wagtail-Flags provides can then be tested like any [Django view](#django-views):

```python
    def test_flags_index(self):
        response = self.client.get('/admin/flags/')
        self.assertEqual(response.status_code, 200)
```

We generally recommend using `WagtailTestUtils` to login and test the admin unless specific user properties or permissions are needed for specific tests.

### Django management commands

When testing custom management commands [Django provides a `call_command()` function](https://docs.djangoproject.com/en/2.1/topics/testing/tools/#management-commands) which will call the command direct the output into a `StringIO` object to be introspected.

For example, our [custom makemessages command (that adds support for Jinja2 templates)](https://github.com/cfpb/consumerfinance.gov/blob/main/cfgov/v1/management/commands/makemessages.py) is tested [using `call_command()`](https://github.com/cfpb/consumerfinance.gov/blob/main/cfgov/v1/tests/management/commands/test_makemessages.py#L66) (although it inspects the resulting `.po` file):

```python
from django.core.management import call_command
from django.test import SimpleTestCase

class TestCustomMakeMessages(SimpleTestCase):
    def test_extraction_works_as_expected_including_jinja2_block(self):
        call_command('makemessages', locale=[self.LOCALE], verbosity=0)

        with open(self.PO_FILE, 'r') as f:
            contents = f.read()

        expected = '''...'''
        self.assertIn(expected, contents)
```

### Feature-flagged code

We use [Django-Flags](https://cfpb.github.io/django-flags/) to feature flag code that should be run under certain conditions, such as waiting for a particular launch window or to A/B test. Sometimes it is necessary to test that code is actually flagged.

In all cases, the easiest way to test with explicit flag states is to [override the `FLAGS` setting](#overriding-settings) and include only your specific flag with a `boolean` condition of `True`.

For example, to test [a function that serves a URL via Wagtail depending on the state of a flag](https://github.com/cfpb/consumerfinance.gov/blob/main/cfgov/cfgov/urls.py#L42-L53), the base URL tests [check behavior with a flag explicitly enabled and again with that flag explicitly disabled](https://github.com/cfpb/consumerfinance.gov/blob/main/cfgov/cfgov/tests/test_urls.py#L92-L109):

```python
class FlaggedWagtailOnlyViewTests(TestCase):

    @override_settings(FLAGS={'MY_TEST_FLAG': [('boolean', True)]})
    def test_flag_set_returns_view_that_calls_wagtail_serve_view(self):
        response = self.client.get('/')
        self.assertContains(
            response,
            'U.S. government agency that makes sure banks'
        )

    @override_settings(FLAGS={'MY_TEST_FLAG': [('boolean', False)]})
    def test_flag_not_set_returns_view_that_raises_http404(self):
        response = self.client.get('/')
        self.assertEqual(response.status_code, 404)
```
