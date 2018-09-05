#Depending on the operating system of the host machines(s) that will build or run the containers, the image specified in the FROM statement may need to be changed.
#For more information, please see https://aka.ms/containercompat

FROM microsoft/dotnet:2.1-aspnetcore-runtime-nanoserver-sac2016 AS base
WORKDIR /app
EXPOSE 56134
EXPOSE 44350

FROM microsoft/dotnet:2.1-sdk-nanoserver-sac2016 AS build
WORKDIR /src
COPY ["RestAPICore.Rest/RestAPICore.Rest.csproj", "RestAPICore.Rest/"]
RUN dotnet restore "RestAPICore.Rest/RestAPICore.Rest.csproj"
COPY . .
WORKDIR "/src/RestAPICore.Rest"
RUN dotnet build "RestAPICore.Rest.csproj" -c Release -o /app

FROM build AS publish
RUN dotnet publish "RestAPICore.Rest.csproj" -c Release -o /app

FROM base AS final
WORKDIR /app
COPY --from=publish /app .
ENTRYPOINT ["dotnet", "RestAPICore.Rest.dll"]