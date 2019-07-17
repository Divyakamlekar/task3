namespace MyTested.AspNetCore.Mvc
{
    using Builders.Contracts.ActionResults.Base;
    using Builders.Contracts.Base;
    using Internal.Contracts.ActionResults;
    using Microsoft.Net.Http.Headers;
    using Utilities.Validators;

    /// <summary>
    /// Contains extension methods for <see cref="IBaseTestBuilderWithContentTypeResult{TContentTypeResultTestBuilder}"/>.
    /// </summary>
    public static class BaseTestBuilderWithContentTypeResultExtensions
    {
        /// <summary>
        /// Tests whether the <see cref="Microsoft.AspNetCore.Mvc.ActionResult"/>
        /// has the same content type as the provided string.
        /// </summary>
        /// <param name="baseTestBuilderWithContentTypeResult">
        /// Instance of <see cref="IBaseTestBuilderWithContentTypeResult{TContentTypeResultTestBuilder}"/> type.
        /// </param>
        /// <param name="contentType">Content type as string.</param>
        /// <returns>The same content type <see cref="Microsoft.AspNetCore.Mvc.ActionResult"/> test builder.</returns>
        public static TContentTypeResultTestBuilder WithContentType<TContentTypeResultTestBuilder>(
            this IBaseTestBuilderWithContentTypeResult<TContentTypeResultTestBuilder> baseTestBuilderWithContentTypeResult,
            string contentType)
            where TContentTypeResultTestBuilder : IBaseTestBuilderWithActionResult
        {
            var actualBuilder = GetActualBuilder(baseTestBuilderWithContentTypeResult);

            ContentTypeValidator.ValidateContentType(
                actualBuilder.TestContext.MethodResult,
                contentType,
                actualBuilder.ThrowNewFailedValidationException);

            return actualBuilder.ResultTestBuilder;
        }

        /// <summary>
        /// Tests whether the <see cref="Microsoft.AspNetCore.Mvc.ActionResult"/> has the
        /// same content type as the provided <see cref="MediaTypeHeaderValue"/>.
        /// </summary>
        /// <param name="baseTestBuilderWithContentTypeResult">
        /// Instance of <see cref="IBaseTestBuilderWithContentTypeResult{TContentTypeResultTestBuilder}"/> type.
        /// </param>
        /// <param name="contentType">Content type as <see cref="MediaTypeHeaderValue"/>.</param>
        /// <returns>The same <see cref="Microsoft.AspNetCore.Mvc.ActionResult"/> test builder.</returns>
        public static TContentTypeResultTestBuilder WithContentType<TContentTypeResultTestBuilder>(
            this IBaseTestBuilderWithContentTypeResult<TContentTypeResultTestBuilder> baseTestBuilderWithContentTypeResult,
            MediaTypeHeaderValue contentType)
            where TContentTypeResultTestBuilder : IBaseTestBuilderWithActionResult
            => baseTestBuilderWithContentTypeResult
                .WithContentType(contentType?.ToString());

        private static IBaseTestBuilderWithContentTypeResultInternal<TContentTypeResultTestBuilder>
            GetActualBuilder<TContentTypeResultTestBuilder>(
                IBaseTestBuilderWithContentTypeResult<TContentTypeResultTestBuilder> baseTestBuilderWithContentTypeResult)
            where TContentTypeResultTestBuilder : IBaseTestBuilderWithActionResult
            => (IBaseTestBuilderWithContentTypeResultInternal<TContentTypeResultTestBuilder>)baseTestBuilderWithContentTypeResult;
    }
     public static class BaseTestBuilderWithFileResultExtensions
    {
        /// <summary>
        /// Tests whether the <see cref="FileResult"/>
        /// has the same file download name as the provided one.
        /// </summary>
        /// <param name="baseTestBuilderWithFileResult">
        /// Instance of <see cref="IBaseTestBuilderWithFileResult{TFileResultTestBuilder}"/> type.
        /// </param>
        /// <param name="fileDownloadName">File download name as string.</param>
        /// <returns>The same <see cref="FileResult"/> test builder.</returns>
        public static TFileResultTestBuilder WithDownloadName<TFileResultTestBuilder>(
            this IBaseTestBuilderWithFileResult<TFileResultTestBuilder> baseTestBuilderWithFileResult,
            string fileDownloadName) 
            where TFileResultTestBuilder : IBaseTestBuilderWithActionResult
        {
            var actualBuilder = GetActualBuilder(baseTestBuilderWithFileResult);
            
            var actualFileDownloadName = GetFileResult(actualBuilder).FileDownloadName;

            if (fileDownloadName != actualFileDownloadName)
            {
                actualBuilder.ThrowNewFailedValidationException(
                    "download name",
                    $"to be {fileDownloadName.GetErrorMessageName()}",
                    $"instead received {(actualFileDownloadName != string.Empty ? $"'{actualFileDownloadName}'" : "empty string")}");
            }

            return actualBuilder.ResultTestBuilder;
        }

        /// <summary>
        /// Tests whether the <see cref="FileResult"/>
        /// has the same last modified value as the provided one.
        /// </summary>
        /// <param name="baseTestBuilderWithFileResult">
        /// Instance of <see cref="IBaseTestBuilderWithFileResult{TFileResultTestBuilder}"/> type.
        /// </param>
        /// <param name="lastModified">Last modified date time offset.</param>
        /// <returns>The same <see cref="FileResult"/> test builder.</returns>
        public static TFileResultTestBuilder WithLastModified<TFileResultTestBuilder>(
            this IBaseTestBuilderWithFileResult<TFileResultTestBuilder> baseTestBuilderWithFileResult,
            DateTimeOffset? lastModified)
            where TFileResultTestBuilder : IBaseTestBuilderWithActionResult
        {
            var actualBuilder = GetActualBuilder(baseTestBuilderWithFileResult);

            var actualLastModified = GetFileResult(actualBuilder).LastModified;

            if (lastModified != actualLastModified)
            {
                actualBuilder.ThrowNewFailedValidationException(
                    "last modified",
                    $"to be {lastModified.GetErrorMessageName()}",
                    $"instead received {actualLastModified.GetErrorMessageName()}");
            }

            return actualBuilder.ResultTestBuilder;
        }

        /// <summary>
        /// Tests whether the <see cref="FileResult"/>
        /// has the same entity tag as the provided one.
        /// </summary>
        /// <param name="baseTestBuilderWithFileResult">
        /// Instance of <see cref="IBaseTestBuilderWithFileResult{TFileResultTestBuilder}"/> type.
        /// </param>
        /// <param name="entityTag">Entity tag header value.</param>
        /// <returns>The same <see cref="FileResult"/> test builder.</returns>
        public static TFileResultTestBuilder WithEntityTag<TFileResultTestBuilder>(
            this IBaseTestBuilderWithFileResult<TFileResultTestBuilder> baseTestBuilderWithFileResult,
            EntityTagHeaderValue entityTag)
            where TFileResultTestBuilder : IBaseTestBuilderWithActionResult
        {
            var actualBuilder = GetActualBuilder(baseTestBuilderWithFileResult);

            var actualEntityTag = GetFileResult(actualBuilder).EntityTag;

            if (Reflection.AreNotDeeplyEqual(entityTag, actualEntityTag))
            {
                actualBuilder.ThrowNewFailedValidationException(
                    "entity tag",
                    $"to be {entityTag.GetErrorMessageName()}",
                    $"instead received {actualEntityTag.GetErrorMessageName()}");
            }

            return actualBuilder.ResultTestBuilder;
        }

        /// <summary>
        /// Tests whether the <see cref="FileResult"/>
        /// has range processing enabled.
        /// </summary>
        /// <param name="baseTestBuilderWithFileResult">
        /// Instance of <see cref="IBaseTestBuilderWithFileResult{TFileResultTestBuilder}"/> type.
        /// </param>
        /// <param name="enableRangeProcessing">Expected boolean value.</param>
        /// <returns>The same <see cref="FileResult"/> test builder.</returns>
        public static TFileResultTestBuilder WithEnabledRangeProcessing<TFileResultTestBuilder>(
            this IBaseTestBuilderWithFileResult<TFileResultTestBuilder> baseTestBuilderWithFileResult,
            bool enableRangeProcessing)
            where TFileResultTestBuilder : IBaseTestBuilderWithActionResult
        {
            var actualBuilder = GetActualBuilder(baseTestBuilderWithFileResult);

            var actualEnableRangeProcessing = GetFileResult(actualBuilder).EnableRangeProcessing;

            if (enableRangeProcessing != actualEnableRangeProcessing)
            {
                actualBuilder.ThrowNewFailedValidationException(
                    "range processing",
                    $"to be {(enableRangeProcessing ? "enabled" : "disabled")}",
                    $"in fact it was {(actualEnableRangeProcessing ? "enabled" : "disabled")}");
            }

            return actualBuilder.ResultTestBuilder;
        }

        private static FileResult GetFileResult<TFileResultTestBuilder>(
            IBaseTestBuilderWithFileResultInternal<TFileResultTestBuilder> baseTestBuilderWithFileResult)
            where TFileResultTestBuilder : IBaseTestBuilderWithActionResult
            => baseTestBuilderWithFileResult
                .TestContext
                .MethodResultAs<FileResult>();

        private static IBaseTestBuilderWithFileResultInternal<TFileResultTestBuilder>
            GetActualBuilder<TFileResultTestBuilder>(
                IBaseTestBuilderWithFileResult<TFileResultTestBuilder> baseTestBuilderWithFileResult)
            where TFileResultTestBuilder : IBaseTestBuilderWithActionResult
            => (IBaseTestBuilderWithFileResultInternal<TFileResultTestBuilder>)baseTestBuilderWithFileResult;
    }
}

}
