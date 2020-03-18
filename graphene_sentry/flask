import json

import sentry_sdk
from flask_graphql import GraphQLView
from graphene_file_upload.flask import FileUploadGraphQLView


class SentryGraphQLView(GraphQLView):
    def dispatch_request(self):
        """Extract any exceptions and send them to Sentry"""
        result = super().dispatch_request()
        try:
            errors = json.loads(result.data.decode('utf-8'))['errors']
            if result and errors:
                self._capture_sentry_exceptions(errors)
        except KeyError:
            pass
        finally:
            return result

    def _capture_sentry_exceptions(self, errors):
        for error in errors:
            sentry_sdk.capture_exception(Exception(error['message']))


class SentryFileUploadGraphQLView(SentryGraphQLView, FileUploadGraphQLView):
    pass
